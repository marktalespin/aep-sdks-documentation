# Configuration methods reference

## SDK configuration

### Launch environment ID

Launch generates a unique environment ID that the SDK uses to retrieve your configuration. This ID is generated when an app configuration is created and published to a given environment. The app is first launched and then the SDK retrieves and uses this Adobe-hosted configuration.

{% hint style="success" %}
We strongly recommend that you configure the SDK with the Launch environment ID.
{% endhint %}

After configuration is retrieved on the app's first launch, it is stored in local cache. Subsequent requests for configuration cause the continued use of the cached configuration, unless configuration changes. If a network error occurs while downloading the configuration file, the local cache is used.

No configuration changes are made when the following conditions are met:

* No change from cached configuration
* Configuration retrieval fails \(due to network or other considerations\)
* A file-read or parsing errors occurs.

The unique environment ID provided by Launch can be configured with the SDK using the following:

{% tabs %}
{% tab title="Android" %}
#### Java

### configureWithAppID <a id="configurewithappid"></a>

#### Syntax <a id="syntax-2"></a>

```java
void configureWithAppID(final String appId)
```

#### Example <a id="example-2"></a>

```java
MobileCore.ConfigureWithAppId("1423ae38-8385-8963-8693-28375403491d");
```
{% endtab %}

{% tab title="iOS" %}
#### Objective-C

### configureWithAppID <a id="syntax-1"></a>

### Syntax <a id="syntax-1"></a>

```objectivec
+ (void) configureWithAppId: (NSString* __nullable) appid;
```

### Example <a id="example-1"></a>

```objectivec
[ACPCore configureWithAppId :@"1423ae38-8385-8963-8693-28375403491d"];
```

{% hint style="info" %}
Alternatively, you may also place the Launch environment ID in your iOS project's _Info.plist_ with the `ADBMobileAppID` key. When the SDK is initialized, the environment ID is automatically read from the _Info.plist_ file, and associated configuration
{% endhint %}
{% endtab %}
{% endtabs %}

## Programmatic updates to configuration

You can also update the configuration programmatically by passing configuration keys and values to override existing configuration. 

{% hint style="info" %}
Keys not found on the current configuration are added when this method is followed. Null values are allowed and will replace existing configuration values.
{% endhint %}

{% tabs %}
{% tab title="Android" %}
#### Java

### updateConfiguration

#### Syntax <a id="syntax-3"></a>

```java
void updateConfiguration(final Map configMap);
```

#### Example <a id="example-3"></a>

```java
HashMap<String, Object> data = new HashMap<String, Object>();
data.put("global.ssl", true);
data.put("global.timezone", "PDT");
data.put("global.timeoneOffset", -420);
MobileCore.updateConfiguration(data);
```
{% endtab %}

{% tab title="iOS" %}
#### Objective-C

### updateConfiguration

#### Syntax

```objectivec
+ (void) updateConfiguration: (NSDictionary* __nullable) config;
```

#### Example

```objectivec
NSMutableDictionary *updatedConfig = [NSMutableDictionary dictionary]; 
[contextData setObject:@"newrsid" forKey:@"analytics.rsid"]; 
[ACPCore updateConfiguration:updatedConfig];
```
{% endtab %}
{% endtabs %}

### Using a bundled file configuration

You can also choose to include a bundled JSON configuration file in your app package to replace or complement by using the [Launch environment ID](./#launch-environment-id) approach.

To download the JSON configuration file, use the following URL:

\`\`[`https://assets.adobedtm.com/PASTE-LAUNCH-ENVIRONMENT-ID.json`](https://assets.adobedtm.com/launch-EN58bc2c40c3b14d688b768fe79a623519-development.json)\`\`

* In iOS, the `ADBMobileConfig.json` file can be placed anywhere that it is accessible in your bundle.
* In Android, the `ADBMobileConfig.json` file must be placed in the assets folder.

You can also load a different `ADBMobileConfig.json` file by using the `ConfigureWithFileInPath` method. The Adobe Experience Platform SDKs will attempt to load the file from the given path and parse its JSON contents. Previous programmatic configuration changes that were set by using the `UpdateConfiguration` method are applied on the bundled file's configuration before setting the new configuration to the Adobe Cloud Platform SDKs. If a file-read error or JSON parsing error occurs, no configuration changes are made.

To pass in a bundled path and file name:

{% tabs %}
{% tab title="Android" %}
#### Java

### configureWithFileInPath <a id="configurewithfileinpath"></a>

#### Syntax <a id="syntax-1"></a>

```java
void configureWithFileInPath(final String filePath)
```

#### Example <a id="example-1"></a>

```java
MobileCore.configureWithFileInPath("absolute/path/to/exampleJSONfile.json");
```
{% endtab %}

{% tab title="iOS" %}
#### Objective-C

### configureWithFileInPath

#### Syntax

```objectivec
+ (void) configureWithFileInPath: (NSString* __nullable) filepath;
```

#### Example

```objectivec
NSString *filePath = [[NSBundle mainBundle] pathForResource:@"ExampleJSONFile"ofType:@"json"]; 
[ACPCore configureWithFileInPath:filePath];
```
{% endtab %}
{% endtabs %}

### Sample configuration

Here's a sample JSON file for the SDK

```javascript
{ 
    "experienceCloud.org": "3CE342C75100435B0A490D4C@AdobeOrg",  
    "target.clientCode": "yourclientcode",  
    "target.timeout": 5,  
    "audience.server": "omniture.demdex.net",  
    "audience.timeout": 5,  
    "analytics.rsids": "mobilersidsample",  
    "analytics.server": "obumobile1.sc.omtrdc.net",  
    "analytics.aamForwardingEnabled": false,  
    "analytics.offlineEnabled": true,  
    "analytics.batchLimit": 0,  
    "analytics.backdatePreviousSessionInfo": false,
    "global.privacy": "optedin",  
    "lifecycle.sessionTimeout": 300,  
    "rules.url": "https://link.to.rules/test.zip"
}
```

