---

copyright:
  years: 2015, 2021
lastupdated: "2021-03-24"

subcollection: assistant-data


---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Building custom applications with the API
{: #api-use}

Use the {{site.data.keyword.discoveryshort}} API to build a custom application or component that searches your data.
{: shortdesc}

For API reference documentation, see [API reference](https://cloud.ibm.com/apidocs/discovery-data){: external}.

## Getting your project ID ![IBM Cloud only](images/ibm-cloud.png)
{: #api-use-cloud}

This information applies to {{site.data.keyword.cloud_notm}} only.
{: important}

To use many of the API methods, you need your project ID.

1.  Open your project in {{site.data.keyword.discoveryshort}}, and then go to the **Integrate and deploy** > **API Information** page.
1.  Copy the project ID. You must specify this value as the `{project_id}` in your API requests.

## Using the API from Cloud Pak for Data ![Cloud Pak for Data only](images/desktop.png)
{: #api-use-cpd}

To use the API, you must construct the URL to use in your requests.

1.  From the {{site.data.keyword.icp4dfull_notm}} web client, go to the details page for the provisioned instance.
1.  Copy the URL from the *Access information* section of the page. You will specify this value as the `{url}`.

    You might want to copy the bearer token also. You will need to pass the token when you make an API call.
1.  From the launched application instance, go to the **Integrate and Deploy** > **View API information** page.
1.  Copy the project ID. You will specify this value as the `{project_id}`.
1.  Construct a request URL by using the IDs you copied. 

    For example, the following request lists the collections in the project:

    ```
    curl -H "Authorization: Bearer {token}" "{url}/v2/projects/{project_id}/collections?version=2019-11-29 -k"
    ```
    {: codeblock}

## API usage ![Cloud Pak for Data only](images/desktop.png)
{: #api-usage}

This information applies to {{site.data.keyword.discovery-data_short}} only.
{: important}

To access the **API usage** page: open the **Projects** page, select **Data usage**, then **API usage**.

This page is used to monitor the usage of the Analyze API, which supports stateless document ingestion workflows. For more information, see [Analyze API](/docs/discovery-data?topic=discovery-data-analyzeapi).

-  **Start date**: The start date of the API call monitoring period.
-  **End date**: The end date of the API call monitoring period.
-  **30-day call total**: This number indicates the number of calls to the Analyze API in the 30-day time interval indicated by the **Start date** and **End date**. The 30-day time interval displayed is determined by calculating the consecutive time period with the highest number of API calls. The 30-day window will update as the time interval with the highest number of API calls changes. 

The **API usage** will not be displayed until some time after API usage monitoring begins. There might be a delay in displaying the final total number of the **30-day call total**, even if the 30-day period listed includes the current date.
{: note}