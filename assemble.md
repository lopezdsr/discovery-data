---

copyright:
  years: 2019, 2021
lastupdated: "2021-03-12"

subcollection: discovery-data

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:important: .important}
{:deprecated: .deprecated}
{:codeblock: .codeblock}
{:screen: .screen}
{:download: .download}
{:hide-dashboard: .hide-dashboard}
{:apikey: data-credential-placeholder='apikey'} 
{:url: data-credential-placeholder='url'}
{:curl: .ph data-hd-programlang='curl'}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:ruby: .ph data-hd-programlang='ruby'}
{:swift: .ph data-hd-programlang='swift'}
{:go: .ph data-hd-programlang='go'}

# Assembling and compiling a custom Cloud Pak for Data connector
{: #assemble}

You package a number of component files together to create a custom connector.
{: shortdesc}

![Cloud Pak for Data only](images/desktop.png) **{{site.data.keyword.icp4dfull_notm}} only**

This information applies only to installed deployments.
{:note}

## Custom connector components
{: #ccs-components}

A custom connector package is a .zip file that contains the following components:

| Path              | Description |
|-------------------|------------|
| `config/template.xml` | A [configuration template](#ccs-config-template)|
| `config/messages.properties` | A [properties file](#ccs-properties-file) for UI messages|
| `lib/*.jar` | [JAR files](#ccs-jar-files) required by the custom connector, not including the connector code that you write|
{: caption=" Connector components" caption-side="top"}

## Configuration template
{: #ccs-config-template}

The configuration template is an XML file that is divided into sections. Each section contains related settings. The XML snippets are taken from the example `template.xml` file whose location is listed in [Understanding the `custom-crawler-docs.zip` file](/docs/discovery-data?topic=discovery-data-connector-dev#ccs-grok-crawler-zip-file).

### Declaration settings
{: #declaration-settings}

Declared settings are represented by the `<declare />` element. The element has the following attributes:

| Attribute name  | Description |
|----------------|-----------|
| `type`          | Data type; one of `string`, `long`, `boolean`, `list` of strings, or `enum`|
| `name`          | The name of the setting|
| `initial-value` | The initial value of the setting|
| `enum-value`    | A list of `enum` values separated by vertical bars (<tt>&#124;</tt>)|
| `required`      | Indicates that the setting is required|
| `hidden`        | Indicates whether to hide the setting from the UI. Specify a value of `true` to hide the setting.|
{: caption="Declare element attributes" caption-side="top"}

  In the current release, the `required` and `hidden` attributes are not applied in the {{site.data.keyword.discoveryshort}} tooling.
  {: note}

### Declaration setting examples
{: #declare-examples}

To declare an `enum` type, use code similar to the following:

```xml
<declare type="enum" name="start_mode" enum-values="NORMAL|QUICK|FULL" initial-value="NORMAL" />
```
{: codeblock}

To declare a hidden `string` with an initial value, use code similar to the following:

```xml
<declare type="string" name="custom_config_class" hidden="true" initial-value="com.example.ExampleCrawlerConfig" />
```
{: codeblock}

To declare a required `long`, use code similar to the following:

```xml
<declare type="long" name="port" required="required" initial-value="22"/>
```
{: codeblock}

### Conditional settings
{: #conditional-settings}

Conditional settings are represented by the `<condition />` element. A conditional setting is displayed only if the condition is satisfied. The element has the following attributes:

| Attribute name  | Description |
|----------------|-----------|
| `name`          | The name of the setting|
| `enable`        | Enable the setting if the value of the `name` attribute equals the value of the `enable` attribute|
| `in`            | Enable the setting if the value of the `name` attribute is included in a specified list of values|
{: caption="Condition element attributes" caption-side="top"}

  In the current release, conditional settings are not applied in the {{site.data.keyword.discoveryshort}} tooling.
  {: note}

### Conditional setting examples
{: #conditional-examples}

To enable a section by using a `boolean` condition, use code similar to the following:

```xml
<declare type="boolean" name="use_key" initial-value="true" />
<!-- Enable setting only if use_key is true -->
<condition name="use_key" enabled="true">
  <declare type="string" name="key" hidden="false" />
</condition>
```
{: codeblock}

To enable a section by using an `enum` condition, use code similar to the following:

```xml
<declare type="enum" name="type" enum-values="PROXY|BASIC|NTLM" initial-value="BASIC"/>
<!-- Enable setting for BAIC, NTLM or PROXY -->
<condition name="type" in="BASIC|NTLM|PROXY">
</condition>
<!-- Enable setting for PROXY -->
<condition name="type" in="PROXY">
```
{: codeblock}

## Template sections
{: #template-sections}

Each section includes one `<declare />` element for each of its settings.

|XPath expression                           |Description          |
|----------------------------------------|---------------------|
|`/function/@name` | The name (type) of the crawler. Not a display name for the UI. Cannot contain spaces.|
|`/function/prototype/proto-section` |A section of the configuration.|
{: caption="Template sections" caption-side="top"}

### Section: `general_settings`
{: #section-general-settings}

The XPath expression for this section is `/function/prototype/proto-section[@section="general_settings"]`. It includes common settings for all crawlers, including the following:

```xml
<declare type="string" name="crawler_name" />
<declare type="string" name="description" />
<declare type="long" name="fetch_interval" initial-value="0" />
<declare type="long" name="number_of_max_threads" initial-value="10" />
<declare type="long" name="number_of_max_documents" initial-value="2000000000" />
<declare type="long" name="max_page_length" initial-value="32768" />
<declare type="enum" name="start_mode" enum-values="NORMAL|QUICK|FULL" initial-value="NORMAL" />
```
{: codeblock}

The custom crawler is initialized with the following settings in the `general_settings` section. For information about the interfaces, see [Developing custom connector code](/docs/discovery-data?topic=discovery-data-connector-dev).

|Name                                  |Value                             |
|--------------------------------------|----------------------------------|
|`custom_config_class` | The name of a class that implements the `com.ibm.es.ama.custom.crawler.CustomCrawlerConfiguration` interface|
|`custom_crawler_class` | The name of a class that implements the `com.ibm.es.ama.custom.crawler.CustomCrawler` interface|
|`custom_security_class` | The name of a class that implements the `com.ibm.es.ama.custom.crawler.CustomCrawlerSecurityHandler` interface|
|`document_level_security_supported`| Specifies whether document-level security is enabled (`true`) or disabled (`false`)|
|`document_level_security_sso_enabled`| Reserved; must be `false`|
|`document_level_security_scope` | A template of the credentials a user must provide to pass filtering. Values that are declared in the `datasource_settings` section can be used as variables.|
{: caption="General settings section defaults" caption-side="top"}

  Document-level security settings are not supported in the current release.
  {: note}

To specify the interfaces, use code similar to the following:

```xml
	    	<!-- Configuration class -->        
            <declare type="string" name="custom_config_class" hidden="true" initial-value="com.ibm.es.ama.custom.crwler.sample.sftp.SftpCrawler" />
	    	<!-- Crawler class -->        
            <declare type="string" name="custom_crawler_class" hidden="true" initial-value="com.ibm.es.ama.custom.crwler.sample.sftp.SftpCrawler" />
	    	<!-- Document level security class -->        
            <declare type="string" name="custom_security_class" hidden="true" initial-value="com.ibm.es.ama.custom.crwler.sample.sftp.SftpCrawler" />
            <!-- Document level security is enabled or not -->
            <declare type="boolean" name="document_level_security_supported" initial-value="true" hidden="true"/>
            <!-- Must be false. -->
            <declare type="boolean" name="document_level_security_sso_enabled" initial-value="false" hidden="true"/>
            <!-- Construct scope from variables in datasource setting. -->
            <declare type="string" name="document_level_security_scope" initial-value="{host}:{port}" hidden="true"/>
```
{: codeblock}

### Section: `datasource_settings`
{: #section-datasource-settings}

The XPath expression for this section is `/function/prototype/proto-section[@section="datasource_settings"]`. It includes settings specific to the data source.

```xml
		<!-- Data source settings change on each server -->
        <proto-section section="datasource_settings">
			<!-- Sample: SFTP server settings -->
            <declare type="string" name="host" required="required" initial-value="localhost"/>
            <declare type="long" name="port" required="required" initial-value="22"/>
            <declare type="string" name="user" required="required" />

			<!-- Sample: Use key file or password -->
            <declare type="boolean" name="use_key" initial-value="true" />
			<!-- Sample: If use key, input key and passphrase -->
            <condition name="use_key" enabled="true">
                <declare type="string" name="key" hidden="false" />
                <declare type="password" name="passphrase" hidden="false" />
            </condition>
            <!-- Sample: If use password, input password -->           
            <condition name="use_key" enabled="false">
                <declare type="password" name="secret_key" hidden="false" />
            </condition>
        </proto-section>
```
{: codeblock}

### Section: `space_filters`
{: #section-space-filters}

The XPath expression for this section is `/function/prototype/proto-section[@section="space_filters"]`. The settings in this section are used to filter the crawl space and need to be hidden, as in the following example from `template.xml`:

```xml
		<!-- Sample: Filter crawl space by root path and level to be recursively check -->    
        <proto-section section="space_filters" hidden="true">
            <declare type="string" name="root_filter" required="required"  initial-value="/" />
            <declare type="long" name="level" required="required" initial-value="1" />
        </proto-section>
```
{: codeblock}

### Section: `crawlspace_settings`
{: #section-crawlspace-settings}

The XPath expression for this section is `/function/prototype/proto-section[@section="crawlspace_settings"]`. The section contains only one `<declare />` element to specify the path. The value of the path is provided by the connector code.

```xml
		<!-- Do not modify, must be here -->    
        <proto-section section="crawlspace_settings" cardinality="multiple">
            <declare type="string" name="path" hidden="true" />
        </proto-section>
```
{: codeblock}

### Section: `plugin_settings`
{: #section-plugin-settings}

The XPath expression for this section is `/function/prototype/proto-section[@section="plugin_settings"]`.

```xml
		<!-- Crawler Plugin is not supported.
        <proto-section section="plugin_settings">
            <declare type="boolean" name="enabled" initial-value="false" />
            <condition name="enabled" enabled="true">
                <declare type="boolean" name="fenced" initial-value="false" hidden="true" />
                <declare type="boolean" name="enabling_for_excluded_contents" initial-value="true" hidden="true" />
                <declare type="string" name="classname" />
                <declare type="string" name="classpath" />
            </condition>
        </proto-section>
        -->
```
{: codeblock}

  This section is not currently supported by the custom connector SDK and its settings are ignored at runtime.
  {: note}

## Properties file
{: #ccs-properties-file}

For an example of a properties file, see the example `messages.properties` file whose location is listed in [Understanding the `custom-crawler-docs.zip` file](/docs/discovery-data?topic=discovery-data-connector-dev#ccs-grok-crawler-zip-file).

## JAR files
{: #ccs-jar-files}

The JAR files for any interfaces used by your custom connector code, including the `ama-zing-custom-crawler-{version_numbers}.jar` file whose location is listed in [Understanding the `custom-crawler-docs.zip` file](/docs/discovery-data?topic=discovery-data-connector-dev#ccs-grok-crawler-zip-file). The `ama-zing-custom-crawler-{version_numbers}.jar` file includes the `com.ibm.es.ama.custom.crawler` Java package that is described in [Developing custom connector code](/docs/discovery-data?topic=discovery-data-connector-dev).

## Compiling and packaging the custom connector
{: #compile-package-connector}

After you write the source code and configuration files for your custom connector, you need to compile and package it.
{: shortdesc}

### Prerequisites
{: #ccs-compilation-prereqs}

To compile a custom connector, you need to have the following items on your local machine. See [Custom connector example](/docs/discovery-data?topic=discovery-data-connector-dev#example-connection-requirements) for details.

  - JDK 1.8 or higher
  - [Gradle](https://gradle.org/install/){: external}
  - The `custom-crawler-docs.zip` file from an installed {{site.data.keyword.discoveryshort}} instance
  - The JSch package
  - The following files for the example custom connector:
    - Java source code (`SftpCrawler.java`)
    - XML definition file (`template.xml`)
    - Properties file (`messages.properties`)

  Do not change the names or paths of the `SftpCrawler.java`, `template.xml`, and `messages.properties` files. Doing so can result in problems including build failures.
  {: important}

### Compiling and packaging the source code
{: #compile-connector}

  1. Ensure you are in the custom connector development directory on your local machine:
     ```sh
     cd {local_directory}
     ```
     {: pre}

  1. Use Gradle to compile your Java source code and to create a .zip file that includes all of the required components for the custom connector:
    ```sh
    gradle build packageCustomCrawler
    ```
    {: pre}

  Gradle creates a file in `{local_directory}/build/distributions/{built_connector_zip_file}`, where the name of the `{built_connector_zip_file}` is based on the value of `initial-value` in line 7 of the `template.xml` definitions file. For example, if the line reads as follows, Gradle outputs a file named `{local_directory}/build/distributions/my-sftp-connector.zip`.

  ```xml
 <declare type="string" name="crawler_name" initial-value="my-sftp-connector"/>
  ```

### Next step
{: #compile-next-step}

Proceed to [Installing and uninstalling a custom connector](/docs/discovery-data?topic=discovery-data-install-connector) to install the custom connector to your {{site.data.keyword.discoveryshort}} instance.
