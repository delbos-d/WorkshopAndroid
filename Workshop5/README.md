![](http://images.androidworld.nl/wp-content/uploads/Android-Training.png)


Workshop Android 5
==================
by delbos-d & DwarfNet
### Objectifs:
* Comprendre le fonctionnement du réseau sur Android.
* Comprendre le fonctionnement d'une API et de la communication avec une API.
* Utiliser Retrofit pour communiquer avec une API :
	* Exécuter des requètes réseaux.
	* Récupérer le contenu d'une réquète.
* Gérer les autorisations d'une application.

### Avant de commencer:
**Récupérer**:
* Le code pour du workshop précédent: [Lien](https://github.com/DwarfNet/Workshop4)
* Le fichers ressource Ressource.W5.zip

* Les fichiers `.java` doivent être dans le dossier `app/src/main/java/{package}/.`
* Les fichiers `.xml` des vues doivent être dans le dossier `app/src/main/res/layout/.`
* Le fichier `strings.xml` doit être dans le dossier `app/src/main/res/values/.`
* Le fichier `AndroidManifest.xml` doit être dans le dossier `app/src/main/.`
* Les fihciers `.png` doivent être dans le dossier `app/src/main/res/drawable/.`
* Les fichiers dans le dossier `libs` doit être à la racine `app/libs/.`

**Bonne Chance!** 

### Ressources:

#### URL
* Source: http://demo5544372.mockable.io
* GET: /workshop
* POST: /workshop
* PUT: /workshop
* DELETE: /workshop

PS: Si tu veux tester avec d'autre url car celle-ci ne marcherons pttr pas vas sur: [lien](https://www.mockable.io) dans Try et crées tes requètes.

### Etapes:

#### Principes
* Serveur web
* API (Application Programming Interface)
* Types de requète (GET / POST / PUT / DELETE)
* Format JSON
* Comunication Mobile <-> API <-> BDD

## Mise en place des outils

#### Faire une vue et une classe pour la requète

*	Details: 
	*	Dans l'Activité lancé au démarrage (du workshop précendent) ajouter un button `brequest_easy` pour lancer la nouvelle activité `RequestRetrofit` (voir Workshop4)
   *	Créer un layout `web_request.xml` avec un button et un textview
   *	Créer une activité (classe) `RequestRetrofit.java` avec les méthode de base `onCreate` / `onClick` / (`setView`)
   *	Déclarer l'activité dans le fichier `AndroidManifest.xml`. 
   *	Afficher le layout `web_request.xml` et initialiser les buttons.

*	Contenu:
	*	Un `Button` dans `activity_main.xml`:
		*	id: brequest_easy
		*	text: "Requete facile"
	*	Un `Button` dans `web_request.xml`:
		*	id: brequest
		*	text: "Executer"
	*	Un `TextView` dans `web_request.xml`:
		*	id: tresult
		*	text: "" (vide)
	
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
        
#### Autoriser la création de socket par l'application
* Details:
	* Ajouter une autorisation `INTERNET` dans le fichier `AndroidManifest.xml`.

Code:

	<?xml version="1.0" encoding="utf-8"?>
	<manifest {...} >
		package= {...}
		<uses-permission android:name="android.permission.INTERNET" />
		<application {...}
		</application>
	</manifest>

## RETROFIT
    
#### Mettre en place Rétrofit

* Détails:
	* Télécharger Retrofit 1.9.
	* Créer un dossier `libs` à la racine de votre projet.
	* Mettre le fichier dans le dossier `libs`.
	* Ajouter Retrofit dans les `dependencies` du `Gradle`.

Code (dans le fichier `build.gradle (Module: app)`):

	dependencies {
		{...}
		compile 'com.squareup.retrofit:retrofit:1.9.0' 
	}

#### Initialiser Rétrofit

* Créer une interface (comme une classe) `ApiTest`.

Code (Interface pour rétrofit):
	
	public interface ApiTest {
	}

* Initialiser rétrofit dans la méthode `onCreate`

Code (Initialisation de Rétrofit 1.9):

	RestAdapter adapter = new RestAdapter.Builder().setEndpoint(voir_ressource_url_source).build();
	ApiTest api = adapter.create(ApiTest.class); //La variable 'api' peut être déclarer en attribut (accessible par toute les méthodes)

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

    api.getTest(new Callback<Response> () {
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

    response.getBody().toString()
    
Code (Exemple pour récupe'rer les message d'erreur)

	error.getMessage()
    
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

    api.postTest("Damien", "Delbos", new Callback<Response> () {
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
    * Afficher un `Toast` en cas d'erreur de la requete avec `Erreur de modification` dans le champs de texte

Code (Exemple de méthode pour déclarer une requète PUT sur rétrofit 1.9)

    @PUT("voir_ressource_url_put/{id}/{value}")
    public void putTest(@Path("id") Integer id, @Path("value") Integer value, Callback<Response> response); //Simple requete PUT avec des paramètres mais pas de réponse en JSON

Code (Exemple d'appel de la réquète)

    api.putTest(1, 42, new Callback<Response> () {
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
    * Afficher un `Toast` en cas d'erreur de la requete avec `Erreur de suppression` dans le champs de texte

Code (Exemple de méthode pour déclarer une requète DELETE sur rétrofit 1.9)

    @DELETE("voir_ressource_url_delete/{id}")
    public void deleteTest(@Path("id") Integer id, Callback<Response> response); //Simple requete DELETE avec des paramètres mais pas de réponse en JSON

Code (Exemple d'appel de la réquète)

    api.deleteTest(42, new Callback<Response> () {
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

    @GET("voir_ressource_url_get/{id}")
    public void getTest2(@Path("id") Integer id, Callback<DataTest> response); //Simple requete GET avec des paramètres et des réponses en JSON

Code (Exemple d'appel de la réquète)

    api.getTest2(10, new Callback<DataTest> () {
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

## ALLER PLUS LOIN 

### Retrofit : vers la 2.0

**Details :**

Pour ceux qui le souhaite, vous pouvez tester la version 2.0 de **retrofit** actuellement en beta. Elle offre entre autre plus de liberté dans les formatages de données, ce qui implique l'utilisation  de `formater` (il n'y en a pas par défault). Elle utilise des mots clef egalement plus parlant.

**ATTENTION!!** Il faut faire attention à la version des tutoriels sur intenet. Depuis environ mi-2015, les post relatif à `retrofit` peuvent etre relatif à la version 1.9 ou 2.0.

Lien de téléchargement de [Retrofit 2.0-beta](http://square.github.io/retrofit/)

Tutoriel pour aider à passer à la 2.0 avec [futurestud.io](https://futurestud.io/blog/retrofit-2-upgrade-guide-from-1-9/) voir completer vos connaissances de Retrofit.

### Retrofit : utilisation des logs

**Details :**

* Ajouter une option lors de la création de du `RestAdapteur`.
* Differents niveau plus ou moins verbeux existent:

 `NONE` < `BASIC` < `HEADERS` < `HEADERS_AND_ARGS` <  `FULL`

Format :
	
	 RestAdapter rest = new RestAdapter.Builder().setEndpoint(Url)
	 						.setLogLevel(RestAdapter.LogLevel.LEVEL_CHOISI)
	 						.build();


### Retrofit : utilisation de Header

**Details :**

Il est possible d'ajouter des headers aux requetes si nécéssaire.

	@Headers({
			// Hearders content
	})
	@GET("url")
	void getRequest(param);

### Android : Verification du réseau

**Details :**

* Ajouter la permission `ACCESS_NETWORK_STATE` dans `AndroidManifest.xml`
* Lors de l'appel de la requete ajouter une verification de l'état du réseau.

Dans `RequestRetrofit.java`:
	
	// Network connection verification.
	ConnectivityManager connMgr = (ConnectivityManager)
	getSystemService(Context.CONNECTIVITY_SERVICE);
	NetworkInfo networkInfo = connMgr.getActiveNetworkInfo();
	if (networkInfo != null && networkInfo.isConnected()) {
		// Get/Post/Put/Delete request
	} else {
		// display error
	}

### Android : le wifi

Page Android Developper sur le [wifi](http://developer.android.com/reference/android/net/wifi/package-summary.html).
Cours du CNAM sur le développement en utilisant le wifi sous Android [lien](http://cedric.cnam.fr/~farinone/RSX116/WiFiAndroid.pdf)

* **Utilisation du wifi**

Ajouter le hardware dans le manifest :

	<manifest {...}>
	<uses-feature android:name="android.hardware.wifi" />
	{...}
	</manifest> 

* **Verifier si on est sur un réseau Wifi ou cellulaire**

Autorisation nécéssaire : `ACCES_WIFI_STATE`

        if (WifiManager.getWifiState() == WifiManager.WIFI_STATE_ENABLED) {
            // Action
        }
        else {
            // Tu veux te connecter ?
        }

* **Connection à un wifi** *(NON TESTÉ)*

Autorisation nécéssaire : `ACCES_WIFI_STATE` et `CHANGE_WIFI_STATE`

	String networkSSID = "SSID_NAME";
	String networkPass = "SSID_PASSPHRASE";

	WifiConfiguration conf = new WifiConfiguration();
	conf.SSID = "\"" + networkSSID + "\"";

Pour les clefs WEP :

	conf.wepKeys[0] = "\"" + networkPass + "\""; 
	conf.wepTxKeyIndex = 0;
	conf.allowedKeyManagement.set(WifiConfiguration.KeyMgmt.NONE);
	conf.allowedGroupCiphers.set(WifiConfiguration.GroupCipher.WEP40);
	
Pour les clefs WPA :

	conf.preSharedKey = "\""+ networkPass +"\"";
	
Pour les réseaux ouvert :
			
	conf.allowedKeyManagement.set(WifiConfiguration.KeyMgmt.NONE);
	
Ensuite, ajouter le réseau à Android Wifi Manager :

	WifiManager wifiManager = (WifiManager)context.getSystemService(Context.WIFI_SERVICE); 
	wifiManager.addNetwork(conf);
	
Enfin, se connecter :

	List<WifiConfiguration> list = wifiManager.getConfiguredNetworks();
	for( WifiConfiguration i : list ) {
   		if(i.SSID != null && i.SSID.equals("\"" + networkSSID + "\"")) {
			wifiManager.disconnect();
			wifiManager.enableNetwork(i.networkId, true);
			wifiManager.reconnect();
			break;
		}
	}

