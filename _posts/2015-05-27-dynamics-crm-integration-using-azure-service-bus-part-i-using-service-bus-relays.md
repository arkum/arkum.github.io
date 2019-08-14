---
layout: post
title: Dynamics CRM Integration using Azure Service Bus - Part I - using Service Bus
  Relays
subtitle: This is the first part of a series of posts about various options for integrating Dynamics CRM with external applications/services
date: '2015-05-27 12:08:00 +1000'
background: 'https://source.unsplash.com/random/300x100'
---
# Dynamics CRM Integration using Azure Service Bus - Part I - using Service Bus Relays

> This is the first part of a series of posts about various options for integrating Dynamics CRM with external applications/services. I will update this space with links to the other posts in the series once I publish them.

Since the release of Dynamics CRM 2011, Dynamics CRM had first class support for talking to azure service bus. Let us look at some of the options for making Dynamics CRM talk to external applications through Azure service Bus. In short we are going to look at how to make CRM talk to external systems using Azure Service Bus Relays, Queues and Topics & Subscriptions.

## Pre-requisites

#### You understand Azure Service Bus Namespaces, Relays, Queues, Topics and Subscriptions

Hmmm, this is hard to explain. Distributed applications using messaging patterns is a broad topic where numerous books were written, lot of money is made by software vendors trying to make it "look" easier, and If I am not mistaken even research papers were published. While it is difficult to explain Azure Service Bus or Distributed Applications using messaging patterns in 15 minutes, If you are completely new to Azure Service Bus this short channel 9 video should help you get started.

#### Azure Service Bus namespace that you are intending to use should have an associated Access Control Service

Until recently an Azure Service Bus namespace you create will automatically be associated with an Access Control Service. However recent changes introduced to Azure portal doesnt create the ACS when you create a Service Bus namespace from the Web portal. However you can use PowerShell to create the namespace and ask azure to create a access control service associated with the namespace

#### Dynamics CRM integration Service Bus Relays

While considering an integration solution that leverages Azure Service Bus Relays, there are two main parts. The service you are exposing to Azure Service Bus using Relay should expose either `IServiceEndpointPlugin` or `ITwoWayServiceEndpointPlugin` interface. Both these interfaces have only one method `Execute` accepting `RemoteExecutionContext` as the sole parameter. As the name of the interfaces implies, `IServiceEndpointPlugin` `Execute` method is one way and does not return anything, while `ITwoWayServiceEndpointPlugin` `Execute` method return a string value. The other end of the solution lies with CRMs ability to talk to these service through Azure Service Bus. This is where things will get easier as you can leverage out of box capabilities exposed by CRM to talk to Azure Service Bus.

There are two ways you can make Dynamics CRM to talk to an external service exposed using Azure Service Bus Relay. One of the obvious choices is to make your plugin talk to Azure Service Bus. However you can also achieve this without writing any code in Dynamics CRM.

##### Exposing a CRM Listener service using Service Relay

Exposing your CRM listener WCF service to Azure is just as same as exposing any other WCF service to Azure using Service Relay, except that the service contracts you expose should be either `IServiceEndpointPlugin` or `ITwoWayServiceEndpointPlugin` as shown below.

It should be obvious from the above code snippet that the entire logic for this service sits inside the Execute method. For e.g as indicated by the comment in the above code snippet, you may want to write some code to call your ERP system and update/insert some records. In order to do whatever processing inside the Execute method, the `RemoteExecutionContext` parameter provides a wrapper for the IPluginExecutionContext (an implementation, to be precise) interface, you would normally use inside a traditional CRM Plug-in. Which means you have access to Pre and Post entity images and its attributes. The above code snippet prints the organization name from the RemoteExecutionContext parameter for demonstration.

Once you have the service implementation defined it is time to expose the service to Azure using Service relay. The below code snippet should work for any WCF service. There is nothing CRM specific here other than the use of IServiceEndpointPlugin interface. The below code snippet does self-hosting of the service in a Console application. However you can always use IIS or any other host.
    
{% highlight csharp %}    
    // Service Bus details
    
    
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.Http;
    string servicenamespace = "";
    string issuerName = "";
    string issuerSecret = "";
    
    Uri address = ServiceBusEnvironment.CreateServiceUri(Uri.UriSchemeHttps, servicenamespace,
    "");
    
    TransportClientEndpointBehavior servicebusbehavior = new TransportClientEndpointBehavior();
    servicebusbehavior.TokenProvider = TokenProvider.CreateSharedSecretTokenProvider
    (issuerName, issuerSecret);
    
    //Configure binding
    WS2007HttpRelayBinding binding = new WS2007HttpRelayBinding();
    binding.Security.Mode = EndToEndSecurityMode.Transport;
    //Configure host
    ServiceHost host = new ServiceHost(typeof(CrmListener));
    host.AddServiceEndpoint(typeof(IServiceEndpointPlugin), binding, address);
    
    //Configure endpoints
    IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);
    foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
    {     endpoint.EndpointBehaviors.Add(serviceRegistrySettings);     endpoint.EndpointBehaviors.Add(servicebusbehavior);
    
    }
    try
    {
    //Open Host     host.Open();
    }
    catch (TimeoutException ex)
    {     Console.WriteLine("Opening of service Timed out");
    }
    Console.ReadLine();
    
{% endhighlight %}
  

  

Once you run the the host application you can see the service endpoint appearing in Azure portal Service Bus –> Relays as shown below. Please note the name "Integrationtest" . This is the name of the endpoint of your service to which Dynamics CRM will talk to. You configure this endpoint while creating the service bus Uri.
    
{% highlight csharp %}    
    Uri address = ServiceBusEnvironment.CreateServiceUri(Uri.UriSchemeHttps, servicenamespace,
                                                        "IntegrationTest");
    
{% endhighlight %}    

![azurerelay][1]

##### Configuring CRM to talk to the service through Azure Service Bus

Once we have the service ready (Service Relay to be technically correct), we have to let CRM know how to talk to this service using Azure Service Bus. This is done by registering a service endpoint using the Plugin Registration Tool. However before starting , head over to CRM online, go to Settings->Customizations->Developer Resources and download the Windows Azure Service Bus Issuer Certificate. You will need this certificate while registering the service endpoint.

![crmcertificate][3]

when you register a service endpoint, the important things to consider are the Path and Contract. Path should exactly match the name of the endpoint you have configured while configuring the Service Relay. In our example, it will be "IntegrationTest". Since our Service implements the one way `IServiceEndpointPlugin` interface, you should select the Contract as "OneWay"

![servieendpoint][4]

After entering these values click Save & Configure ACS button and the tool will prompt you for the certificate and the issuer name. Browse to the certificate you have downloaded earlier and give the issuer name as "crm.dynamics.com". Once you click configure and Save the settings, Plugin Registration tool will automatically create the appropriate service identity settings for your service bus namespace in Azure. Once this configuration is complete you can head over to Azure management portal and verify the settings under the Access Control Service portal for your Namespace.

![acsserviceidentity][5]

Next step is to register a step under the Service End point registration you have just created using the Plugin Registration Tool. Select the Service End point and from the Register Menu click Register New Step menu item. In the below screen I am registering a step to post the execution context to the registered Service Endpoint whenever an opportunity is updated.

![registernewstep][6]

Please note that Dynamics CRM will not allow you to select Execution Mode as Synchronous. You can test the integration by performing an update operation on opportunity entity. Make sure your service is running before you update the opportunity entity. If your service is not hit or you get any error while updating the opportunity, you can check the status of the integration in Dynamics CRM under Settings –> System Jobs.

#### Writing a two-way service

From the above example it should be clear that if you are using the out of box CRM support for communicating with Azure Service Bus, you will be following a fire and forget style communication with your service. What if you want to do some processing with the return value from the external service in your Dynamics CRM? At the services side you have to implement `ITwoWayServiceEndpointPlugin` interface. The following code shows a service which returns a simple string to the caller.

In order to host the service I have avoided the lengthy WCF configuration code we did in the previous sample and moved all the configuration to the App.config file
{% highlight csharp %}   
    
    ServiceHost host = null;
    try
    {
        host = new ServiceHost(typeof(CrmTwoWayListener));
        host.Open();
        Console.WriteLine("Service Started");
        Console.WriteLine("Press Enter to stop the service");
        Console.ReadLine();
    }
    catch (Exception ex)
    {
        Console.WriteLine(ex);
    }
    finally
    {
        host.Close();
    }
{% endhighlight %} 

As before, if you run your service you can see the service appearing under the Service Relay in your Azure Portal under your Service Bus namespace.

Once the service is up and running, you have to create a Plugin to talk to it using the SDK. This is relatively straightforward. The IServiceProvider container passed to the Execute method of the IPlugin interface, contains an instance of the IServiceEndpointNotificationService, which has one method Execute. Execute method expects you to pass an entity reference to the "serviceendpoint" system entity corresponding to the Service Endpoint you register in the Plugin Registration Tool. Which means you have to have the unique id of the Service Endpoint registration record, which you can grab it from the Plugin Registration tool and pass it as a configuration string to the Plugins constructor as shown in the code snippet below.
    
{% highlight csharp %}
    public class ContactUpdatePlugin : IPlugin
    {
        private Guid serviceendpointid;
        public ContactUpdatePlugin(string config)
        {
            if(string.IsNullOrEmpty(config) || !Guid.TryParse(config,out serviceendpointid)){
                throw new InvalidPluginExecutionException("Invalid Plugin Configuration");
            }
        }
        public void Execute(IServiceProvider serviceProvider)
        {
            IPluginExecutionContext context = (IPluginExecutionContext)serviceProvider.GetService(typeof(IPluginExecutionContext));
            ITracingService trace = (ITracingService)serviceProvider.GetService(typeof(ITracingService));
            IServiceEndpointNotificationService azureservice = (IServiceEndpointNotificationService)serviceProvider.GetService(typeof(IServiceEndpointNotificationService));
    
            Entity entity = (Entity)context.InputParameters["Target"];
            if(entity == null) throw new InvalidPluginExecutionException("Target Entity is null");
    
            IOrganizationService orgservice = (IOrganizationService)serviceProvider.GetService(typeof(IOrganizationService));
    
            if (orgservice == null)
            {
                IOrganizationServiceFactory serviceFactory =  (IOrganizationServiceFactory)serviceProvider.GetService(typeof(IOrganizationServiceFactory));
                if (serviceFactory == null) throw new InvalidPluginExecutionException("org service factory is null");
                orgservice = serviceFactory.CreateOrganizationService(context.UserId);
                if (orgservice == null) throw new InvalidPluginExecutionException("org service is null");
            }
    
            string response = null;
            try
            {
                response = azureservice.Execute(new EntityReference("serviceendpoint", serviceendpointid), context);
    
            }
            catch(Exception ex)
            {
                trace.Trace("Exception while calling external service {0}", ex.ToString());
                response = ex.Message;
            }
            try
            {
    
                var serviceContext = new OrganizationServiceContext(orgservice);
                if (serviceContext == null) throw new InvalidPluginExecutionException("Service Context is null");
                Entity post = new Entity("post");
                post["regardingobjectid"] = new EntityReference(entity.LogicalName, entity.Id);
                post["text"] = response;
                post["source"] = new OptionSetValue(1);
                post["type"] = new OptionSetValue(7);
    
                serviceContext.AddObject(post);
                serviceContext.SaveChanges();
            }
            catch (Exception ex)
            {
                if (ex.InnerException != null) throw new InvalidPluginExecutionException("Error updating post n" + ex.InnerException.Message);
                throw new InvalidPluginExecutionException("Error updating post n" + ex.Message);
            }
        }
    }
{% endhighlight %}    

To make things little more interesting I am creating a post entity with the return value from the service and attaching to the original contact entity so that we can verify the results side by side. Even if the external service returns an exception it will appear in the post. Registering this Plugin is same as registering any other Plugin except that you have to remember to update the secured and unsecured configuration strings with the unique id of the Service Endpoint registration. Please note that Dynamics CRM will allow you to register this as a Synchronous Plugin which means that until the external service returns or Plugin times out your operation will not complete.

![aftercontactupdate][7]

I will cover Dynamics CRM integration using Azure Service Bus Queues and Topics in the coming posts in this series.

[1]: /uploads/2015/05/azurerelay_thumb.png "azurerelay"
[2]: /uploads/2015/05/wlEmoticon-smile.png
[3]: /uploads/2015/05/crmcertificate_thumb.png "crmcertificate"
[4]: /uploads/2015/05/servieendpoint_thumb.png "servieendpoint"
[5]: /uploads/2015/05/acsserviceidentity_thumb.png "acsserviceidentity"
[6]: /uploads/2015/05/registernewstep_thumb.png "registernewstep"
[7]: /uploads/2015/05/aftercontactupdate_thumb.png "aftercontactupdate"

  