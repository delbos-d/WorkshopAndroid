![](http://images.androidworld.nl/wp-content/uploads/Android-Training.png)


Workshop Android 5
==================
### Objectifs:
* Comprendre le fonctionnement du réseau sur Android
* Comprendre le fonctionnement d'une API et de la communication avec une API
* Exécuter des requètes réseaux
* Récupérer le contenu d'une réquète
* Utiliser Retrofit pour communiquer avec une API

### Avant de commencer:
**Récupérer** le code pour du workshop précédent: [Lien](https://github.com/DwarfNet/Workshop4)
* Les fichiers `.java` doivent être dans le dossier `app/src/main/java/{package}/.`
* Les fichiers `.xml` des vues doivent être dans le dossier `app/src/main/res/layout/.`
* Le fichier `strings.xml` doit être dans le dossier `app/src/main/res/values/.`
* Le fichier `AndroidManifest.xml` doit être dans le dossier `app/src/main/.`
* Les fihciers `.png` doivent être dans le dossier `app/src/main/res/drawable/.`

**Bonne Chance!** 

### Ressources:

#### URL
* Source: http://demo5544372.mockable.io
* GET: /workshop
* POST: /workshop
* PUT: /workshop
* DELETE: /workshop

PS: Si tu veux tester avec d'autre url car celle-ci ne marcherons pttr pas vas sur: [lien](www.mockable.io) dans Try et crées tes requètes.

### Etapes:

#### Principes
* Serveur web
* API (Application Programming Interface)
* Types de requète (GET / POST / PUT / DELETE)
* Format JSON
* Comunication Mobile <-> API <-> BDD

#### Faire une vue et une classe pour la requète

*	Details: 
    *	Créer un layout `web_request.xml` avec un button et textview
    *	Créer une activité (classe) `RequestEasy.java` avec les méthode de base `onCreate` / `onClick` / (`setView`)
    *	Déclarer l'activité dans le fichier `AndroidManifest.xml`
    *	Dans l'Activité lancé au démarrage (du workshop précendent) ajouter un button `brequest_easy` pour lancer la nouvelle activité `RequestEasy` (voir Workshop4)
    *	Afficher le layout `web_request.xml` et initialiser les buttons.

*	Contenu:
	*	Un `Button`
		*	id: brequest
		*	text: "Executer"
	*	Un `TextView`
		*	id: tresult
		*	text: "" (vide)
	*	Un `Button`
		*	id: brequest_easy
		*	text: "Requete facile"
	
Code (Exemple d'Activité):   

    public class MainActivity extends Activity implements View.OnClickListener {
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setView(LAYOUT_ID);
        }
    
        private void setView(int view) {
            if (view == LAYOUT_ID) {
                setContentView(view);
            }
        }
    
        @Override
        public void onClick(View v) {
            //if (v.getId() == id_to_check) {}
        }
    }

Code (Exemple de déclaration d'actvité dans `AndroidManifest.xml`):

    <activity
        android:name=".views.ActivityName"
        android:label="@string/app_name"
        android:theme="@style/AppTheme" >
    </activity>
        
#### Executer une requete simple (POST)
    
*	Details: 
    *	Créer une méthode (fonction) `requestPost` avec un argument `String url`
    *	Appeler la méthode `request` avec un clique sur `brequest`

Code (Exemple d'appel POST):

	Thread t = new Thread(new Runnable() 
	{
		public void run() 
		{
			try
	        {
	        	HttpPost httpvar = new HttpPost("URL");
	    		DefaultHttpClient httpclient = new DefaultHttpClient();
	            HttpResponse response = httpclient.execute(httpvar);
			} 
			catch (ClientProtocolException e) {e.printStackTrace();} 
			catch (IOException e) {e.printStackTrace();} 
			catch (Exception e) {e.printStackTrace();}
		}
	});
	t.start();
	try {t.join();} catch (InterruptedException e) {e.printStackTrace();}
	
#### Récupérer le contenu d'une requète

* Détails:
    * Afficher dans `tresult` le résultat d'une requète

Code (Récupérer le contenu d'une requète):

    HttpResponse response = httpclient.execute(httpvar);
    BufferedReader reader = new BufferedReader(new InputStreamReader(response.getEntity().getContent(), "UTF-8"));
    String result = reader.readLine();

Code (Documentation pour changer le contenu d'un TextView): [Lien](http://developer.android.com/reference/android/widget/TextView.html#setText\(java.lang.CharSequence\))

#### Executer une requete plus complexe

* Détails:
    * Envoyer des paramètres à la requète POST
* Contenu:
    * Paramètre: `firstname`, valeur: votre prénom
    * Paramètre: `secondname`, valeur: votre nom

Code (Ajouter des paramètres à une requète):

    HttpPost httpvar = new HttpPost("URL");
    varForm = new ArrayList<NameValuePair>();
	varForm.add(new BasicNameValuePair("PARAMETER_NAME", "PARAMETER_VALUE")); //A dupliquer
	httpvar.setEntity(new UrlEncodedFormEntity(varForm, "UTF-8"));
    HttpResponse response = httpclient.execute(httpvar);

#### Executer une requete GET

* Détails:
    * Dupliquer la méthode `requestPost` en créant la méthode `requestGet`
* Contenu:
    * Remplacer dans la méthode `requestGet` tous le contenu du `try catch` par le système de requète Get

Code (requète Get):

    HttpGet httpget = new HttpGet(URL);
    ResponseHandler<String> responseHandler = new BasicResponseHandler();
    SetServerString = Client.execute(httpget, responseHandler);
    
#### Mettre en place Rétrofit

* Détails:
    * Télécharger Retrofit 1.9
    * Créer un dossier `libs` à la racine de votre projet et mettez la librairie dedant
    * Créer une interface (comme un classe) `ApiTest` vide

Code (Interface pour rétrofit)

    import retrofit.http.GET;
    
    public interface ApiAeroclubManager {
    }

#### Initialiser Rétrofit

* Détails:
    * Créer une activité (classe) `RequestRetrofit.java` avec les méthode de base `onCreate` / `onClick` / (`setView`)
    * Dans l'Activité lancé au démarrage (du workshop précendent) ajouter un button `brequest_retrofit` pour lancer la nouvelle activité `RequestRetrofit` (voir Workshop4)
    * Initialiser rétrofit dans la méthode `onCreate`
    * Déclarer l'activité dans le fichier `AndroidManifest.xml`
    * Afficher le layout `web_request.xml` et initialiser les buttons.

Code (Initialisation de Rétrofit 1.9)

    RestAdapter adapter = new RestAdapter.Builder().setEndpoint(voir_ressource_url_source).build();
    ApiTest api = adapter.create(ApiTest.class); //La variable api peut être déclarer en attribut (accessible par toute les méthodes)

#### Utiliser rétrofit (GET)

* Détails:
    * Déclarer une méthode dans l'interface `ApiTest` pour définir une requète GET simple
    * Créer une méthode private `requestGet` sans paramètre, dans la Classe courante, et faire un appel de la requète définit
    * Exécuter la méthode `requestGet` dans la méthode `onCreate` aprés l'affichage de la vue et remplir le `tresult` avec le résultat
    * Afficher un `Toast` en cas d'erreur de la requete avec `Erreur de récupération` dans le champs de texte

Code (Exemple de méthode pour déclarer une requète GET sur rétrofit 1.9)

    @GET("voir_ressource_url_get")
    public void getTest(Callback<Response> response); //Simple requete GET sans paramètre ni réponse en JSON
    //Cette déclaration suffi pour indiquer à rétrofit comment il doit traiter l'url

Code (Exemple d'appel de la réquète)

    api.getTest(new CallBack<Response> () {
            @Override
            public void success(Response response, Response response2) {
                //Code éxecuté en cas de succes
            }

            @Override
            public void failure(RetrofitError error) {
                //Code éxecuté en cas d'erreur
            }
        });
        
Code (Exemple pour récupérer le contenu d'une page)

    response.getBody().toString();
    
Code (Exemple de `Toast`)

    Toast.makeText(this, "Mon message", Toast.LENGTH_SHORT).show();

#### Utiliser rétrofit (POST)
* Détails:
    * Déclarer une méthode dans l'interface `ApiTest` pour définir une requète POST simple
    * Créer une méthode private `requestPost` sans paramètre, dans la Classe courante, et faire un appel de la requète définit
    * Exécuter la méthode `requestPost` dans la méthode `onCreate` aprés l'affichage de la vue et remplir le `tresult` avec le résultat
    * Afficher un `Toast` en cas d'erreur de la requete avec `Erreur de récupération` dans le champs de texte

Code (Exemple de méthode pour déclarer une requète POST sur rétrofit 1.9)

    @POST("voir_ressource_url_post")
    public void postTest(@Query("fisrtname") String firstName, @Query("secondname") String secondName, Callback<Response> response); //Simple requete POST avec des paramètres mais pas de réponse en JSON
    //Cette déclaration suffi pour indiquer à rétrofit comment il doit traiter l'url

Code (Exemple d'appel de la réquète)

    api.postTest("Damien", "Delbos", new CallBack<Response> () {
            @Override
            public void success(Response response, Response response2) {
                //Code éxecuté en cas de succes
            }

            @Override
            public void failure(RetrofitError error) {
                //Code éxecuté en cas d'erreur
            }
        });
        
#### Utiliser rétrofit (PUT)
* Détails:
    * Déclarer une méthode dans l'interface `ApiTest` pour définir une requète PUT simple `putTest`
    * Créer une méthode private `requestPut` sans paramètre, dans la Classe courante, et faire un appel de la requète définit
    * Exécuter la méthode `requestPut` dans la méthode `onCreate` aprés l'affichage de la vue et remplir le `tresult` avec le résultat
    * Afficher un `Toast` en cas d'erreur de la requete avec `Erreur de récupération` dans le champs de texte

Code (Exemple de méthode pour déclarer une requète PUT sur rétrofit 1.9)

    @PUT("voir_ressource_url_put/{id}/{value}")
    public void putTest(@Path("id") Integer id, @Path("value") Integer value, Callback<Response> response); //Simple requete PUT avec des paramètres mais pas de réponse en JSON

Code (Exemple d'appel de la réquète)

    api.putTest(1, 42, new CallBack<Response> () {
            @Override
            public void success(Response response, Response response2) {
                //Code éxecuté en cas de succes
            }

            @Override
            public void failure(RetrofitError error) {
                //Code éxecuté en cas d'erreur
            }
        });
#### Utiliser rétrofit (DELETE)
* Détails:
    * Déclarer une méthode dans l'interface `ApiTest` pour définir une requète DELETE simple `deleteTest`
    * Créer une méthode private `requestDelete` sans paramètre, dans la Classe courante, et faire un appel de la requète définit
    * Exécuter la méthode `requestDelete` dans la méthode `onCreate` aprés l'affichage de la vue et remplir le `tresult` avec le résultat
    * Afficher un `Toast` en cas d'erreur de la requete avec `Erreur de récupération` dans le champs de texte

Code (Exemple de méthode pour déclarer une requète DELETE sur rétrofit 1.9)

    @DELETE("voir_ressource_url_delete"/{id})
    public void deleteTest(@Path("id") Integer id, Callback<Response> response); //Simple requete DELETE avec des paramètres mais pas de réponse en JSON

Code (Exemple d'appel de la réquète)

    api.deleteTest(42, new CallBack<Response> () {
            @Override
            public void success(Response response, Response response2) {
                //Code éxecuté en cas de succes
            }

            @Override
            public void failure(RetrofitError error) {
                //Code éxecuté en cas d'erreur
            }
        });
        
#### Récupérer une réponse en JSON

* Détails:
    * Créer une classe `DataTest` avec 3 attributs (variables) public `firstname`, `secondname`, `value` et rien d'autre
    * Déclarer une méthode dans l'interface `ApiTest` pour définir une requète GET simple `getTest2`
    * Afficher toutes les variable de la classe `DataTest` en cas de success
    * Afficher un `Toast` en cas d'erreur de la requete avec `Erreur de récupération` dans le champs de texte

* Contenu:
    * Attribut `firstname`
        * Type: `String`
    * Attribut `secondname`
        * Type: `String`
    * Attribut `value`
        * Type: `Integer`

Code (Exemple de méthode pour déclarer une requète GET sur rétrofit 1.9)

    @GET("voir_ressource_url_get"/{id})
    public void getTest2(@Path("id") Integer id, Callback<DataTest> response); //Simple requete GET avec des paramètres et des réponses en JSON

Code (Exemple d'appel de la réquète)

    api.getTest2(10, new CallBack<DataTest> () {
            @Override
            public void success(DataTest data, Response response2) {
                //Code éxecuté en cas de succes
                //String firstname = data.firstname; //contient la valeur de la chaine récupéré en JSON dans la page retourné par l'API
            }

            @Override
            public void failure(RetrofitError error) {
                //Code éxecuté en cas d'erreur
            }
        });
