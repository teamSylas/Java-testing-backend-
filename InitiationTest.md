# Pour ce projet de test nous utilisons JUnit et de Mockito
## Initiation 
> chaque classe de text doit etre préceder de l'anotation  
``` @ExtendWith(MockitoExtension.class) ```  
### Les Mock
Les mocks ne conserne pas la méthode qu'on veux tester , mais seulement les elements qui entre en ligne de compts dans le traitement de la methode a texter.
Des exemple de Mock pour les classes utilliser dans l'application :
```
@Mock
	private Context contextMock;
	
@Mock
	private DomainObject domainObjectMock  
  ```  
 ### Les Mock Statique (pour mocké les class Static)  
 > MockedStatic<"*nom de la classe*"> contextUtil = Mockito.mockStatic(*nom de la classe*.class)  
 Exemple du mock de la class static **ContextUtil**   
 ```MockedStatic<ContextUtil> contextUtil = Mockito.mockStatic(ContextUtil.class);```  

### 
  
### Tester un une méthode 
```diff
- Pour tester une méthode il faut la mettre tous en ``` public void ```  
+ Rechercher les méthode a mock pour tester la méthode a tester en question 
+ Passer en mode débugage pour retrouver les méthode à mock si on a du mal 
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
### Mock d'une méthode **void**
on Commence pas les mots clé :
```diff
+ lenient() pour éviter que le test échout trop à cause d'un effet de bort.
+ doNothing() pour dire que la méthode de renvoie rien.
```
Puis on applle la classe et on accompagne la méthode et on lui passe les arguments qu'il faut.
Exemple : 
```lenient().doNothing().when(dARefTableDao).checkoutFiles(ArgumentMatchers.isNull(), ArgumentMatchers.any(DARefTable.class), ArgumentMatchers.anyString());```

### Mock d'une Méthode dont la classe est static  
#### **car simple**  
> <"**Instance du mock de la class** ">.when(() -> "**Instance du mock de la class**"."**déthode à mocké**"("**les arguments de ma méthode**")).thenReturn("**le résultat que doit retourné l'appelle de la méthode**");
Exemple : 
```
domainObject.when(() -> DomainObject.findObjects(ArgumentMatchers.any(Context.class), ArgumentMatchers.anyString(), ArgumentMatchers.anyString(), ArgumentMatchers.anyString(),ArgumentMatchers.any(), ArgumentMatchers.any(), ArgumentMatchers.any(), ArgumentMatchers.anyBoolean(), ArgumentMatchers.any())).thenReturn(refTableList);
```
#### **Mock d'une méthode *voide* d'une classe statique**
On garde le même comportement mais c'est dans la partie thenAnswer que tout change 
```diff
! Answer<Void> renvoie une répons void .
! invocation -> null et revoie null a ton invocation.
```
Exemple: 
```
contextUtil.when(() -> ContextUtil.pushContext(contextMock)).thenAnswer((Answer<Void>) invocation -> null);
```
## Tester la partie API
Dans ce car il faut avoir un instance du serveur dans la classe ou l'API veut être tester. Dans notre explemple 
- on garde toujours l'anotion : *@ExtendWith(MockitoExtension.class)* 
- puis on regle de problement de l'appelle au contexte avec notre classe statique
- on dicque les classe a mock pour fait fonctionner notre API pui on initialise le server
- ensuit on test l'appelle a API pour cela : 
	- on crée un client, 
	- on indique le type (JSON) consommer 
	- on presise les parametre 
	- on indique le type de requet *.request().get()*
	- on test en tout premier le type de reponse du server 
	- on recuper le contenue de la répons qu'on a produite nous même 
	- checke du résultat
Exemple :
### GET 
```ruby
@ExtendWith(MockitoExtension.class)
public class AttributeControllerTest {

	static class AttributeControllerMock extends AttributeController {

		@Override
		public Context getAuthenticatedContext(HttpServletRequest request, boolean arg1) {
// Return null because the Context will be not used. In fact, the businessService method that require the Context will be mocked, because it is not tested here
			return null;
		}
	}
	/**
	 * The 'Business Service' Layer is not tested here. So we mock it
	 */
	@Mock
	private AttributeServices attributeServicesMock;

	/**
	 * REST API controller under test. The annotation @InjectMocks indicates to Mockito this is the class to inject the mocks into
	 */
	@InjectMocks
	private AttributeControllerMock attributeControllerMock;

	@RegisterExtension
	final JaxrsServerExtension<AttributeControllerMock> server = JaxrsServerExtension.newInstance(AttributeControllerMock.class, () -> attributeControllerMock).addProvider(new JacksonJsonProvider());
	
	// [----Call Web Service----]
		final Response response = ClientBuilder.newClient() // create new Client
				.register(new JacksonJsonProvider()) // with Jackson JSON provider
				.target(server.getBaseUrl()) // On server specified by its URL
				.path("Attributes/attributes/rangesWithTranslation").queryParam("attributeName", ATTRIBUTE_NAME).request().get();

	// [----Check WS result------]
		assertEquals(200, response.getStatus());
	// [------Récuperation du contenue de la répons---------]
	final List<RangeItem> actualRange = response.readEntity(new GenericType<List<RangeItem>>() {
		});
	//[-------cheque du contenue de la répons-------]
		assertEquals(expectedRangeItems.size(), actualRange.size());

```

### POST
- au niveau du post en fonction de ce qui est passer on peur utiliser serializer ou un déserializer 
- pu remarque la fin de la tranmission qui different d'un *get*

```java
	// Call Web Service
		final Response response = ClientBuilder.newClient() // create new Client
				.register(new JacksonJsonProvider().configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false)) // with Jackson JSON provider
				.target(server.getBaseUrl()) // On server specified by its URL
				.path(path).request().post(Entity.entity(expectedResult, MediaType.APPLICATION_JSON));

		// Check WS result
		assertEquals(200, response.getStatus());

		DACoDesign actual = response.readEntity(DACoDesign.class);
```

### put 
```Java 
		// Call Web Service
		final Response response = ClientBuilder.newClient() // create new Client
				.register(new JacksonJsonProvider()) // with Jackson JSON provider
				.target(server.getBaseUrl()) // On server specified by its URL
				.path(path).request().put(Entity.entity(wsArgs, MediaType.APPLICATION_JSON));

		// Check WS result
		assertEquals(200, response.getStatus());
```
  
