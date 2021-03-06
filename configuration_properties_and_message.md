## Configuration Properties and Messages
Convention over configuration - put `application.properties` & `message.properties` in resource folder & it will work   


`application.properties` : **related to configuration**    
`message.properties` : **related to i18n messages/texts/String literals (localization)**    

profile specific configuration - `application-{profileName}.properties` i.e.
```
application-dev.properties
application-stage.properties
application-production.properties
```
 - `application.properties` is main & will always be loaded.    
 - Profile specific configuration properties will override `application.properties`    

**To set active profile** open `application.properties` & add:
```
spring.profiles.active= dev

```

locale specific message - `message_localeName.properties` i.e.
```
message_en.properties
message_jp.properties
message_bn.properties
```
To enable custom name for message, open `application.properties` & add:
```
# i18n messages
spring.messages.basename= messages, server_base_uri
```

## Using Configuration Properties

**Setting Configuration Property Value to a Field of Component/Service/Repository**
```
public class MyComponent{ // or Service/Repository

	@Value("${myprefix.id}")
	private int id; // value (222) for key 'myprefix.id' will be assigned to id
	
}
```
**[More Details about Using Configuration Properties](https://github.com/hovermind/springboot-webmvc/blob/master/using_configuration_properties.md)**    

## Getting Message Using Key
```
public class MyService{ // or Component/Repository

	@Autowired
	private MessageSource messageSource;

	public String getMessageForKey(String key){
	
		// getMessage(String key, String[] placeHolderValues, String defaultValue, Locale locale)
	
		return messageSource.getMessage(key, null, "default", Locale.JAPAN);
	}
}
```
**PlaceHolderValues in Message**    
`message.properties`
```
myprefix.salt= $#-{0}-222-{1}
```
`{0}` & `{1}` will be replaced by PlaceHolderValues
```
	@Autowired
	private MessageSource messageSource;

	public String getMessageWithPlaceHolder(String key, String placeHolderValues){
	
		return messageSource.getMessage(key, placeHolderValues, "default", Locale.JAPAN);
	}
```
If we call `getMessageWithPlaceHolder("myprefix.salt", new String[]{"my", "secret"})` then we will get `"$#-my-222-secret"`


