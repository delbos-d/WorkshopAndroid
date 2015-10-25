![](http://images.androidworld.nl/wp-content/uploads/Android-Training.png)


Workshop Android 4
==================
### Objectifs:
* Créer des vues
* Agir sur le clique de bouton
* Gérer des champs de textes
* Manipuler l'affichage d'image
* Manipuler l'affichage des objects dans une view


### Avant de commencer:
**Récupérer** le code du workshop précédent, formater : [Lien](https://github.com/delbos-d/WorkshopAndroid/blob/master/Workshop4/Ressouce.W4.zip).

**Bonne Chance!** 

### Etapes:

#### Creation d'une vue (xml)

*	Details: Clique droit dossier res/layout + "New" + "Layout ressource file", "activity_main.xml"
*	Contenu:
	*	Un `Bouton`
		*	id: bform
		*	text: "Formulaire"
	*	Un `Bouton`
		*	id: bfun
		*	text: "Fun"
	
Code (Exemple de bouton):   

	<Button
		android:layout_width="match_parent"  
		android:layout_height="wrap_content"     
		android:text="Mon bouton" 				
		android:id="@+id/bmybutton" />

#### Initialisation des vues (java)
*	Détails: Créer une méthode `setView` avec une ressource en paramètre qui ne retourne rien. Cette méthode vérifie par des `if` ou des `switch & case` le type de vue et charge la vue (layout) et initialise les différents éléments (ex: boutons). Penser à remplacer le `setContentView` dans méthode `onCreate` par `setView`. 
*	Contenu:
	*	Condition1: Si il s'agit du layout `activity_main.xml`.
	*	Action1:
		*	Appliquer le layout `activity_main`.
		*	Initialiser le clique du bouton `bForm` et `bFun`.
	*	Condition2: Si il s'agit du layout `activity_form.xml`.
	*	Action2:
		*	Appliquer le layout `activity_form`.
		*	Initialiser le clique du bouton `bPrint` et `bBack`.

Code (Prototype de la méthode):

	private void setView(int view) {
		if (view == R.layout.activity_main) {
		}
		else if (view == R.layout.activity_form) {
		}
	}

Code (Appliquer un layout à une activité):

	setContentView(R.layout.LAYOUT_NAME) //Vous pouvez utiliser la variable envoyer en paramètre de `setView`.

Code (A ajouter à la suite de la classe):

	implements View.OnClickListener
	//par exemple public class MainActivity extends Activity implements View.OnClickListener
	//Cette implémentation demande l'utilisation de la méthode 

Code (Initialiser l'action d'un clique d'un bouton):

	((Button) findViewById(R.id.ID_OF_BUTTON)).setOnClickListener(this);

#### Appliquer une action aux boutons pour changer les vues (java)
*	Détails: Dans la méthode `onClick` changer de vue lors du clique sur les boutons `bForm` et `bBack`.
*	Contenu:
	*	Condition1: Si l'id de la vue cliqué est le bouton `bForm`
	*	Action1:
		*	Changer la vue, avec `setView`, par le layout `activity_form`.
	*	Condition2: Si l'id de la vue cliqué est le bouton `bBack`
	*	Action2:
		*	Changer la vue, avec `setView`, par le layout `activity_main`.

Code (Méthode qui reçoit les cliques): <a id="onClick"></a>

	@Override
    public void onClick(View v) {
        if (v.getId() == R.id.ID_OF_BUTTON) {
            
        }
    }

Code (Changement de layout avec `setView`);

	setView(R.layout.LAYOUT_NAME);

### → Lancer votre simulateur!

#### Appliquer une action a un bouton pour valider le contenu d'un champ (java)
*	Détails: Lors du clique sur le bouton `bPrint` afficher une popup avec le contenu des champs texte du formulaire.
*	Contenu:
	*	Condition1: Si l'id de la vue cliqué est le bouton `bPrint`
	*	Action1:
		*	Récupérer le contenu des champs du formulaire `eMail` & `ePass`
		*	Afficher une popup avec la valeur des champs récupérer.

Code (Récupérer le contenu des champs de texte):

	String value = ((EditText) findViewById(R.id.ID_OF_EDITTEXT)).getText().toString();

Code (Concaténer des strings):

	String result = value1 + value2 + "value 3";

Code (Afficher une popup):

	new AlertDialog.Builder(this)
		.setTitle("Your Title")
        .setMessage("You message")
        .setPositiveButton("OK", null)
        .show();

#### Vérifier le contenu de champs texte automatiquement (java)
*	Détails: Faire une fonction qui prend une `String` en paramètre et retourne un `boolean`. Ajouter la vérification d'un champs à chaque nouvelle lettre.
*	Contenu:
	*	Créer une fonction `checkEmail` qui prend une `String` en paramètre et retourne un `boolean`. Celle-ci doit vérifier la taille de la chaine en paramètre (`>=` 4 et `<=` 15) et retourne le résultat.
	*	Appeler la fonction à chaque changement de caracètre.

Code (Fonction de vérification d'email):

	public static boolean checkEmail(String value) {
		if (value.length() < 4 || value.length() > 15)
			return (false);
		/*
		//Check the email validity using pattern
		Pattern pattern = Patterns.EMAIL_ADDRESS;
        if (!pattern.matcher(value).matches());
			return (false);
		*/
		return (true);
	}

Code (Pour appeler une methode a chaque ajout de caractère dans un champs):

	((EditText) findViewById(R.id.ID_OF_EDITTEXT)).addTextChangedListener(new TextWatcher() {
        public void afterTextChanged(Editable s) {
            if (!fonctionDeCheck(s.toString())) {
                ((EditText) MainActivity.this.findViewById(R.id.ID_OF_EDITTEXT)).setError("Invalid value");
            } else {
                ((EditText) MainActivity.this.findViewById(R.id.ID_OF_EDITTEXT)).setError(null);
            }
        }
        public void beforeTextChanged(CharSequence s, int start, int count, int after) {}
        public void onTextChanged(CharSequence s, int start, int before, int count) {}
    });

### → Lancer votre simulateur!

#### Créer un autre layout (xml)
*	Détails: Créer une vue `activity_fun.xml` avec un `LinearLayout` et ajouter un `ImageView` en caché, un bouton `bAbra` visible et `bNext` en caché.
*	Contenu:
	*	Un `ImageView`
		*	id: image
		*	src: @drawable/android
		*	visibility: GONE
	*	Un `Button`
		*	id: `bAbra`
		*	text: "Abracadabra"
	*	Un `Button`
		*	id: `bNext`
		*	text: "Next"
		*	visibility: GONE

Code (Exemple d'ImageView):

	<ImageView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/image"
        android:layout_gravity="center_horizontal"
        android:visibility="gone" />

#### Créer une autre activité (java)
*	Détails: Créer une autre classe `FunActivity` et charger la vue `activity_fun` et initialiser les boutons.
*	Contenu:
	*	Une classe `FunActivity`
		*	`extends` de `Activity`
		*	`implements` `View.OnClickListener`
		*	Surcharge une méthode `onCreate`
		*	Surcharge une méthode `onClick` comme vu précedemment
		*	Implémenter `setView` comme vu précedemment
		*	Charger le layout `activity_fun` dans `onCreate` avec `setView`
		*	Initialiser les boutons `bNext` & `bAbra` comme vue précédement

Code (Surcharger la méthode `onCreate`)

	@Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
       //Set the view here
    }

#### Afficher une image lors d'un clique (java)
*	Détails: Lors du clique sur le bouton `bAbra` cacher ce bouton et faire apparaitre l'image et l'autre bouton `bNext`.
*	Contenu:
	*	Condition1: Clique sur le bouton `bAbra`
	*	Action1:
		*	Changer la visibilité du bouton `bAbra` à Button.GONE
		*	Changer la visibilité du bouton `bNext` à Button.VISIBLE
		*	Changer la visibilité de l'image `image` à ImageView.VISIBLE
	*	Condition2: Clique sur le bouton `bNext`
	*	Action2:
		*	Changer l'image de `image`

Code (Changer la visibilité d'un élément):

	((Button) findViewById(R.id.BUTTON_ID)).setVisibility(Button.VISIBLE); //It could be VISIBLE / INVISIBLE / GONE
	//For other object you just change the class

Code (Changer une image):

	((ImageView) findViewById(R.id.IMAGEVIEW_ID)).setImageRessource(R.drawable.IMAGE_ID);

### → Lancer votre simulateur!

#### "BONUS" Afficher une image aléatoire a chaque clique de bouton (java)
*	Détails: Avoir qu'un seul bouton (suprimer `bNext`) et afficher l'image a partir du premier clique et à chaque clique afficher une image différente (aléatoirement).
*	Contenu:
	*	Supprimer le bouton `bNext`
	*	Faire apparaitre une image aléatoire à chaque clique sur le bouton `bAbra`.

Code (Générer un chiffre aléatoire):

	Random rand = new Random();
	int maximum_value = 4;
	int random_value = rand.newInt(max + 1); //Le résultat: [0;max]

### → Lancer votre simulateur!