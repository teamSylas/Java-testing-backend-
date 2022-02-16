# Pour ce projet de test nous utilisons JUnit et de Mockito
## Initiation 
> chaque classe de text doit etre préceder de l'anotation  
``` @ExtendWith(MockitoExtension.class) ```  
### Les Mock
Les mocks ne conserne pas la méthode qu'on veux tester , mais seulement les elements qui entre en ligne de compts dans le traitement de la methode a texter.
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
  
### Tester un une méthode
```diff
- Pour tester une méthode il faut la mettre tous en ``` public void ```  
+ Rechercher les méthode a mock pour tester la méthode a tester en question 
+ Passer en mode débugage pour retrouver les méthode à mock si on a du mal 
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
 
 ### Mock d'une Méthode dont la classe est static  ??  
**car simple**  
> <"**Instance du mock de la class** ">.when(() -> "**Instance du mock de la class**"."**déthode à mocké**"("**les arguments de ma méthode**")).thenReturn("**le résultat que doit retourné l'appelle de la méthode**");
  

  
  
