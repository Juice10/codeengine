---

copyright:
  years: 2021
lastupdated: "2021-03-11"

keywords: configmaps with code engine, secrets with code engine, key references with code engine, key-value pair with code engine, setting up secrets with code engine, setting up configmaps with code engine

subcollection: codeengine

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:api: .ph data-hd-interface='api'}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: data-hd-programlang="c#"}
{:cli: .ph data-hd-interface='cli'}
{:codeblock: .codeblock}
{:curl: .ph data-hd-programlang='curl'}
{:deprecated: .deprecated}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:fuzzybunny: .ph data-hd-programlang='fuzzybunny'}
{:generic: data-hd-operatingsystem="generic"}
{:generic: data-hd-programlang="generic"}
{:gif: data-image-type='gif'}
{:go: .ph data-hd-programlang='go'}
{:help: data-hd-content-type='help'}
{:hide-dashboard: .hide-dashboard}
{:hide-in-docs: .hide-in-docs}
{:important: .important}
{:ios: data-hd-operatingsystem="ios"}
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
{:note .note}
{:note: .note}
{:objectc data-hd-programlang="objectc"}
{:org_name: data-hd-keyref="org_name"}
{:php: data-hd-programlang="php"}
{:pre: .pre}
{:preview: .preview}
{:python: .ph data-hd-programlang='python'}
{:python: data-hd-programlang="python"}
{:route: data-hd-keyref="route"}
{:row-headers: .row-headers}
{:ruby: .ph data-hd-programlang='ruby'}
{:ruby: data-hd-programlang="ruby"}
{:runtime: architecture="runtime"}
{:runtimeIcon: .runtimeIcon}
{:runtimeIconList: .runtimeIconList}
{:runtimeLink: .runtimeLink}
{:runtimeTitle: .runtimeTitle}
{:screen: .screen}
{:script: data-hd-video='script'}
{:service: architecture="service"}
{:service_instance_name: data-hd-keyref="service_instance_name"}
{:service_name: data-hd-keyref="service_name"}
{:shortdesc: .shortdesc}
{:space_name: data-hd-keyref="space_name"}
{:step: data-tutorial-type='step'}
{:subsection: outputclass="subsection"}
{:support: data-reuse='support'}
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:tutorial: data-hd-content-type='tutorial'}
{:ui: .ph data-hd-interface='ui'}
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vbnet: .ph data-hd-programlang='vb.net'}
{:video: .video}


# Setting up and using secrets and configmaps 
{: #configmap-secret} 

Learn how to work with secrets and configmaps in {{site.data.keyword.codeengineshort}}. In {{site.data.keyword.codeengineshort}}, you can store your information as key-value pairs in secrets or configmaps that can be consumed by your job or application by using environment variables. 
{: shortdesc}

## What are secrets and configmaps and why would I use them? 
{: #configmapsec-whatwhy}

In {{site.data.keyword.codeengineshort}}, both secrets and configmaps are key-value pairs and when mapped to environment variables, are set such that they have a name that corresponds to the "key" of each entry in those maps, and the value of the environment variable is the "value" of that key.

A configmap provides a method to include non-sensitive data information to your deployment. By referencing values from your configmap as environment variables, you can decouple specific information from your deployment and keep your app or job portable. A configmap contains information in key-value pairs.

A secret provides a method to include sensitive configuration information, such as passwords or SSH keys, to your deployment. By referencing values from your secret, you can decouple sensitive information from your deployment to keep your app or job portable. Anyone who is authorized to your project can also view your secrets; be sure that you know that the secret information can be shared with those users. Secrets contain information in key-value pairs.

Since secrets and configmaps are similar entities (except secrets are stored more securely), the way you interact and work with secrets and configmaps is also similar.  

You can work with configmaps and secrets with the CLI, but this capability is not currently available from the console.
{: important} 

## Creating configmaps
{: #configmap-create}

Create configmaps with {{site.data.keyword.codeengineshort}}.
{: shortdesc}



### Creating a configmap with the CLI
{: #configmap-create-cli}

Create configmaps with the  {{site.data.keyword.codeengineshort}} CLI.
{: shortdesc}

You can populate a configmap in several ways. You can populate it by specifying the key-value pairs directly on the command line, or you can point to a file. 

**Before you begin**
   * Set up your [{{site.data.keyword.codeengineshort}} CLI](/docs/codeengine?topic=codeengine-install-cli) environment.
   * [Create and work with a project](/docs/codeengine?topic=codeengine-manage-project).

When you create (or update) a configmap from a file, the format must be `--from-file FILE` or `--from-file KEY=FILE`. In {{site.data.keyword.codeengineshort}}, when you use a file to specify configmap values, *all* of the contents within the file become the value for the key-value pair. When you use the option format of `--from-file KEY=FILE`, the `KEY` is name of the environment variable that is known to your job or app. When you use the option format of `--from-file FILE`, `FILE` is the name of the environment variable that is known to your job or app. 
{: important}

1. Create a configmap with the `configmap create` command in one of the following ways: 

    * Create a configmap directly on the command line by using the `--from-literal` option. For example,

        ```
        ibmcloud ce configmap create --name myliteralconfigmap --from-literal TARGET=yellow
        ```
        {: pre}

    * Create a configmap by pointing to a file that contains the key-value pair by using the `--from-file` option. For this example, let's use a file that is named `configmapcolors.txt`, which contains the text `blue, green, red`. The following example command uses the `--from-file KEY=FILE` format with the `configmap create` command: 

        ```
        ibmcloud ce configmap create --name mycolorconfigmap --from-file TARGET=configmapcolors.txt
        ```
        {: pre}

2. Now that the configmap is created, use the `configmap list` command to list all configmaps in your project or use the `configmap get` command to display details about a specific configmap. For example,

    ```
    ibmcloud ce configmap get --name mycolorconfigmap
    ```
    {: pre}

   **Example output**
   
   ```
    Getting configmap 'mycolorconfigmap'...
    OK

    Name:          mycolorconfigmap
    ID:            abcdefgh-abcd-abcd-abcd-1a2b3c4d5e6f
    Project Name:  myproject
    Project ID:    01234567-abcd-abcd-abcd-abcdabcd1111
    Age:           11s
    Created:       2020-10-14 14:10:57 -0400 EDT

    Data:
    ---
    TARGET: blue, green, red
   ```
   {: screen}

## Using configmaps 
{: #configmap-using}

After you define a configmap, your jobs or apps can consume and use the information that is stored in the configmap. 
{: shortdesc}



### Using configmaps with the CLI 
{: #configmap-using-cli}

Use defined configmaps with jobs or apps. Let's use the configmaps that were previously defined with the CLI with an application.

1. [Deploy an app](/docs/codeengine?topic=codeengine-application-workloads#deploy-app-cli). For this example, create an app that uses the [`Hello`](https://hub.docker.com/r/ibmcom/hello) image in Docker Hub. When a request is sent to this sample app, the app reads the environment variable `TARGET` and prints `Hello ${TARGET}`. If this environment variable is empty, `Hello World` is returned. 

    ```
    ibmcloud ce app create --name myhelloapp --image ibmcom/hello 
    ```
    {: pre}

   When you call the `myhelloapp` app, the output is `Hello World`.  

2. Update the app to use the previously defined `mycolorconfigmap` configmap. 

    From the CLI, to use a defined configmap with an application, specify the `--env-from-configmap` option on the `app create` or `app update` CLI commands. Similarly, to use a configmap with a job, specify the `--env-from-configmap` option on the `job create`, `job update`, `jobrun submit`, or `jobrun resubmit` CLI commands. 
    {: tip}

    ```
    ibmcloud ce app update --name myhelloapp --env-from-configmap mycolorconfigmap
    ```
    {: pre}

    **Example output**
   
   ```
    Updating application 'myhelloapp' to latest revision.
    [...]
        Run 'ibmcloud ce application get -n myhelloapp' to check the application status. 
    OK

    https://myhelloapp.d484a5d6-d10d.us-south.codeengine.appdomain.cloud
   ```
   {: screen}

3. Call the application again. This time, the app returns `Hello blue, green, red`, which is the value that is specified in the `mycolorconfigmap` configmap.

    ```
    curl https://myhelloapp.d484a5d6-d10d.us-south.codeengine.appdomain.cloud
    ```
    {: pre}

   **Example output**
   
   ```
    Hello blue, green, red
   ```
   {: screen}

4. Update the app again to use the `myliteralconfigmap` configmap. 

   When you update an application or job with an environment variable that fully references a configmap to fully reference a different configmap, full references override other full references in the order in which they are set (the last referenced set overrides the first set).
   {: note}

    ```
    ibmcloud ce app update --name myhelloapp --env-from-configmap myliteralconfigmap
    ```
    {: pre}

   **Example output**
   
   ```
    Updating application 'myhelloapp' to latest revision.
    [...]
    Run 'ibmcloud ce application get -n myhelloapp' to check the application status.
    OK 

    https://myhelloapp.d484a5d6-d10d.us-south.codeengine.appdomain.cloud
   ```
   {: screen}

5. Call the application again. This time, the app returns `Hello yellow` which is the value that is specified in the `myliteralconfigmap` configmap.

    ```
    curl https://myhelloapp.d484a5d6-d10d.us-south.codeengine.appdomain.cloud
    ```
    {: pre}

   **Example output**
   
   ```
    Hello yellow
     ```
   {: screen}

6. To change the value of a key-value pair in a configmap, use the `configmap update` command. Let's update the `myliteralconfigmap` configmap to change the value of the `TARGET` key from `Sunshine` to `Stranger`.

    ```
    ibmcloud ce configmap update --name myliteralconfigmap --from-literal "TARGET=Stranger"
    ```
    {: pre}

7. Run the `app update` command to restart the application; for example: 

    ```
    ibmcloud ce app update --name myhelloapp
    ```
    {: pre}

    If you update a secret or configmap that is referenced by an app, you must restart your app for the new data to take effect. Use the `app update` command to restart your app; for example, `ibmcloud ce app update --name myapp`, where `myapp` is the name of your app. This condition applies for secrets and configmaps that are referenced as environment variables. This condition doesn't apply if your app [references secrets or configmaps as mounted files](/docs/codeengine?topic=codeengine-secretcm-reference-mountedfiles).
{: important}

8. Call the application again. This time, the app returns `Hello Stranger`, which is the updated value that is specified in the `myliteralconfigmap` configmap.

    ```
    curl https://myhelloapp.d484a5d6-d10d.us-south.codeengine.appdomain.cloud
    ```
    {: pre}

   **Example output**
   
   ```
    Hello Stranger
   ```
   {: screen}

To summarize, you completed basic scenarios to demonstrate how to use configmaps with an app by referencing a full configmap and updating keys within a configmap.

For more detailed scenarios about referencing full secrets and configmaps as environment variables, overriding references, and removing references in the CLI, see [Referencing secrets and configmaps](/docs/codeengine?topic=codeengine-secretcm-reference).


## Creating secrets
{: #secret-create}

Use secrets to provide sensitive information to your apps or jobs. Secrets are defined in key-value pairs and the data that is stored in secrets is encoded.
{: shortdesc}



### Creating a secret with the CLI
{: #secret-create-cli}

Learn how to create generic secrets that can be consumed by jobs or apps as environment variables.

With the CLI, you can create a secret where the data is pulled from a file, or specified directly  with the `secret create` command. Secrets that are used to store simple name-value pairs are called *generic secrets*. Secrets that store information about how to authenticate to a container registry are called [registry access secrets (`imagePullSecret`)](/docs/codeengine?topic=codeengine-plan-image).

You can populate a secret in several ways. For example, you can populate it by specifying the key-value pairs directly on the command line or you can point to a file. 

**Before you begin**

   * Set up your [{{site.data.keyword.codeengineshort}} CLI](/docs/codeengine?topic=codeengine-install-cli) environment.
   * [Create and work with a project](/docs/codeengine?topic=codeengine-manage-project).

When you create (or update) a secret from a file, the format must be `--from-file FILE` or `--from-file KEY=FILE`. In {{site.data.keyword.codeengineshort}}, when you use a file to specify secret values, *all* of the contents within the file is the value for the key-value pair. When you use the option format of `--from-file KEY=FILE` the `KEY` is name of the environment variable that is known to your job or app. When you use the option format of `--from-file FILE`, `FILE` is the name of the environment variable that is known to your job or app.
{: important}

1. Create a generic secret with the `secret create` command in one of the following ways:   

    * Create a secret directly from the command line by using the `--from-literal` option. For example, 

        ```
        ibmcloud ce secret create --name myliteralsecret --from-literal "TARGET=My literal secret"
        ```
        {: pre}

    * Create a secret by pointing to a file that contains the key-value pair by using the `--from-file` option. For this example, use a file that is named `secrets.env`, which contains `my little secret1`. 
    
        * The following example uses the `--from-file KEY=FILE` format with the `secret create` command:  

            ```
            ibmcloud ce secret create --name mysecretmsg --from-file TARGET=secrets.env
            ```
            {: pre}

        * The following example command uses the `--from-file FILE` format with the `secret create` command. In this example, `TARGET` (no extension) is the name of the file, which is the same as the name of the environment variable that is known to the job.

            ```
            ibmcloud ce secret create --name mysecretmsg2  --from-file TARGET
            ```
            {: pre}

2. Now that secrets are created, use the `secret list` command to list all secrets in your project or use the `secret get` command to display details about a specific secret. For example,

    ```
    ibmcloud ce secret get --name mysecretmsg2
    ```
    {: pre}

   **Example output**
   
   ```
    Getting generic secret 'mysecretmsg2'...
    OK

    Name:          mysecretmsg2
    ID:            abcdefgh-abcd-abcd-abcd-c88e2775388e
    Project Name:  myproject
    Project ID:    01234567-abcd-abcd-abcd-abcdabcd1111
    Age:           9s
    Created:       2020-10-14 14:12:55 -0400 EDT

    Data:
    ---
    TARGET: bXkgYmlnIHNlY3JldDI=
   ```
   {: screen}

Notice that the value of the key `TARGET` for this secret is encoded.  

## Using secrets 
{: #secret-using}

After you define your secret, your jobs or apps can consume and use the information that is stored in the secret. 
{: shortdesc}



### Using secrets with the CLI 
{: #secret-using-cli}

Use defined secrets with jobs or apps. Let's use secrets that were previously defined with the CLI with a job.

To use a secret with a job with the CLI, specify the `--env-from-secret` option on the `job create`, `job update`, `jobrun submit`, or `jobrun resubmit` commands. Similarly, to reference a secret from an application with the CLI, specify the `--env-from-secret` option on the `app create` or `app update` commands.  

This scenario uses the CLI to run a job that references a secret. 

1. [Create and run a job](/docs/codeengine?topic=codeengine-job-deploy). For this example, create a {{site.data.keyword.codeengineshort}} job that uses the [`testjob`](https://hub.docker.com/r/ibmcom/testjob) image in Docker Hub and then run the job. When a request is sent to this sample job, the job reads the environment variable `TARGET` and prints `"Hello ${TARGET}!"`. If this environment variable is empty, `"Hello World!"` is returned. 

    ```
    ibmcloud ce job create --name myjob --image ibmcom/testjob --array-indices 1-5
    ```
    {: pre}

2. Run the `myjob` job. 

    ```
    ibmcloud ce jobrun submit --name myjobrun --job myjob
    ```
    {: pre}

3. Display the logs of the `myjobrun` job run. You can display logs of all of the instances of a job run or display logs of a specific instance of a job run. In this example, the job run log displays the output of `Hello World!`. Use the `jobrun get` command to display the details of the job run, including its running instances. The following command displays the logs of `myjobrun-2-0` (the second instance of this job run) where `myjobrun` is the name of the job run, `2` is the second instance of the job run, and the `0` is the `retryindex` value of the job run.

    ```
    ibmcloud ce jobrun logs --instance myjobrun-2-0
    ```
    {: pre}

   **Example output**
   
   ```
    Getting logs for job run instance 'myjobrun-2-0'...
    OK

    myjobrun-2-0:
    Hello World!
   ```
   {: screen}

4. Update the configuration of the job to reference the previously defined `mysecretmsg` secret. 

    ```
    ibmcloud ce job update --name myjob --env-from-secret mysecretmsg
    ```
    {: pre}

5. Run the updated `myjob` job. The name of the job run must be unique. 

    ```
    ibmcloud ce jobrun submit --name myjobrun2 --job myjob
    ```
    {: pre}

6.  Use the `jobrun get` command to display details of the job run, including the instances of the job run. 

    ```
    ibmcloud ce jobrun get --name myjobrun2
    ```
    {: pre}

   **Example output**
   
   ```
    Getting jobrun 'myjobrun2'...
    Getting instances of jobrun 'myjobrun2'...
    Getting events of jobrun 'myjobrun2'...

    Name:               myjobrun2

    [...]

    Running Instances:
        Name           Ready  Status     Restarts  Age
        myjobrun2-1-0  0/1    Succeeded  0         4m
        myjobrun2-2-0  0/1    Succeeded  0         4m
        myjobrun2-3-0  0/1    Succeeded  0         4m
        myjobrun2-4-0  0/1    Succeeded  0         4m
        myjobrun2-5-0  0/1    Succeeded  0         4m
   ```
   {: screen}

7. Display the logs of the `myjobrun2` job run. You can display logs of all of the instances of a job run or display logs of a specific instance of a job run. This time, display the logs of the all the instances of the job run. The log displays `Hello my little secret1!`, which was specified by using an environment variable with a secret.

    ```
    ibmcloud ce jobrun logs --jobrun myjobrun2
    ```
    {: pre}

   **Example output**
   
   ```
    Getting logs for all instances of job run 'myjobrun2'...
    Getting jobrun 'myjobrun2'...
    Getting instances of jobrun 'myjobrun2'...
    OK

    myjobrun2-1-0:
    Hello my little secret!

    myjobrun2-2-0:
    Hello my little secret!

    myjobrun2-3-0:
    Hello my little secret!

    myjobrun2-4-0:
    Hello my little secret!

    myjobrun2-5-0:
    Hello my little secret!
   ```
   {: screen}

8. Resubmit the job run and specify to use the `myliteralsecret` secret for this job run.  

   When you update a job or app with an environment variable that fully references a secret to fully reference a different secret, full references override other full references in the order in which they are set (the last referenced set overrides the first set).
   {: note}

    ```
    ibmcloud ce jobrun resubmit  --jobrun myjobrun2  --name myjobrun2resubmit  --env-from-secret myliteralsecret
    ```
    {: pre}

9. Display the job run logs of an instance of the `myjobrun2resubmit` job run. This time, display the logs of the fourth instance of the job run. The log displays `Hello My literal secret!!`, which is the value that is specified in the `myliteralsecret` secret. Use the `jobrun get` command to display the details of the job run, including the running instances of the job run. 

    ```
    ibmcloud ce jobrun logs --instance myjobrun2resubmit-4-0
    ```
    {: pre}

   **Example output**
   
   ```
   Getting logs for job run instance 'myjobrun2resubmit-4-0'...
   OK

   myjobrun2resubmit-4-0/myjob:    
   Hello My literal secret!
   ```
   {: screen}

10. To change the value of key-value pair in a secret, use the `secret update` command. Let's update the `myliteralsecret` secret to change the value of the `TARGET` key from `My literal secret` to `My new literal secret`.

    ```
    ibmcloud ce secret update --name myliteralsecret --from-literal "TARGET=My new literal secret"
    ```
    {: pre}

11. Resubmit the job run again and specify to use the `myliteralsecret` secret for this job run. 

    ```
    ibmcloud ce jobrun resubmit  --jobrun myjobrun2  --name myjobrun2resubmit-b --env-from-secret myliteralsecret 
    ```
    {: pre}

12. Display the logs of the `myjobrun2resubmit-b` job run. This time, the job log displays `Hello My new literal secret!`, which is the value in the updated `myliteralsecret` secret. You can use the `jobrun get` command to  display the details of the job run, including the running instances of the job run. Display the logs for any running instance of the job run.

    ```
    ibmcloud ce jobrun logs --instance myjobrun2resubmit-b-5-0
    ```
    {: pre}

   **Example output**
   
   ```
   Getting logs for job run instance 'myjobrun2resubmit-b-5-0'...
   OK

   myjobrun2resubmit-b-5-0/myjob:    
   Hello My new literal secret!
   ```
   {: screen}

To summarize, you completed basic scenarios to demonstrate how to use secrets with a job by referencing a full secret and updating keys within a secret.

For more detailed scenarios about referencing full secrets and configmaps as environment variables, overriding references, and removing references in the CLI, see [Referencing secrets and configmaps](/docs/codeengine?topic=codeengine-secretcm-reference).


## Deleting secrets and configmaps 
{: #configmapsecret-delete}

When you no longer need a configmap or secret, you can delete it. 
{: #shortdesc} 



### Deleting secrets and configmaps with the CLI
{: #configmapsecret-delete-cli}

* To delete a configmap with the CLI, use the [`configmap delete`](/docs/codeengine?topic=codeengine-cli#cli-configmap-delete) command; for example, 

    ```
    ibmcloud ce configmap delete --name myliteralconfigmap -f
    ```
    {: pre}

    **Example output**

    ```
    Deleting configmap 'myliteralconfigmap'...
    OK
    ```
    {: screen}

* To delete a secret with the CLI, use the [`secret delete`](/docs/codeengine?topic=codeengine-cli#cli-secret-delete) command; for example, 

    ```
    ibmcloud ce secret delete --name myliteralsecret -f
    ```
    {: pre}

    **Example output**

    ```
    Deleting secret myliteralsecret...
    OK
    ```
    {: screen}

## Next steps
{: #next-steps-configmapsecret}

When you work with secrets and configmaps in the CLI, you can reference full secrets and configmaps or you can reference individual keys in secrets and configmaps. For more detailed scenarios about referencing full secrets and configmaps as environment variables, overriding references, and removing references in the CLI, see [Referencing secrets and configmaps with environment variables](/docs/codeengine?topic=codeengine-secretcm-reference).

You can also reference secrets and configmaps as mounted files. For more information, see [Referencing secrets and configmaps as mounted files](/docs/codeengine?topic=codeengine-secretcm-reference-mountedfiles).
