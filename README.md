Plataphorma
===========

Meteor pet project created to teach my students the following Meteor functionality: 

* Collection's publish/subscribe 
* Deps.autorun 
* Meteor.methods/call 
* Integration of non-Meteor code in compatibility folder (HTML5 games Alien Invasion and Froot Wars)
* Usage of allow to control client access to collections

![ScreenShot](/screenshot.png)


Plataphorma offers the possibility to run 2 different HTML5 games: Alien Invasion and Froot Wars. 

On the right side of the screen the best players of each game are shown, updated in real time each time a signed in player finishes a game. If no game is selected, the best players overall are shown.

On the left side of the screen a chatroom for the current game is available. Only signed in users can post messages. If no game is selected a general chatroom is shown.

The original code of the two HTML5 games integrated in this project is available here:
* Alien Invasion: https://github.com/cykod/AlienInvasion
* Froot Wars: http://www.wrox.com/WileyCDA/WroxTitle/Professional-HTML5-Mobile-Game-Development.productCd-1118301323,descCd-DOWNLOAD.html

Bootstrap style (file bootstrap.min.css) provided by http://bootswatch.com


Running the project
-------------------

A live version of this code is running here: http://plataphorma.meteor.com

To run the project locally, clone the repo and run ```meteor``` inside it. You can see in .meteor/packages that this Meteor project uses these packages:
* ```meteor remove autopublish```
* ```meteor remove insecure```
* ```meteor add bootstrap```
* ```meteor add accounts-ui```
* ```meteor add accounts-password```
-------------------------------------------------------------------------------------
Práctica 8.
-------------------------------------------------------------------------------------
1. Click en botón de juego.

        Es a partir de la línea 112 de client.js lo que se ejecuta cuando le das a click en uno de los juegos. 
        $('#container').hide(); oculta lo que se mostraba previamente 
        $('#gamecontainer').show(); muestra el juego actual 
        var game = Games.findOne({name:"AlienInvasion"}); encuentra un juego en la base de datos que se llame en este caso "AlienInvasion"
        Session.set("current_game", game._id); cambia la sesión al juego actual.
        Al cambiar la sesión al darle click aparecen los mensajes y las mejores puntuaciones del juego sobre el que has dado click que están guardadas y al darle click a alguno de los juegos cambian por las de ese juego.

2. Se escribe un mensaje en el chat sin estar autenticado.
        Cuando se escribe un mensaje al no estar autenticado el usuario se ejecuta la línea 101 $("#login-error").show(); que es la que ejecuta lo que hay en el html con esa id. 
        <div class="alert fade in" id="login-error" style="display:none;">
        <button type="button" class="close">x</button>
                You must be signed in to post messages.
        </div>
        Se crea un botón con una x que al darle se cierra.

3. Se escribe un mensaje en el chat estando autenticado.

        Al darle al intro se comprueba que el usuario esté autenticado if (Meteor.userId()), se crean las variables con el id y el mensaje var user_id = Meteor.user()._id; var message = $('#message'); y si el mensaje no es nulo, se inserta en la base de datos Messages.insert({
                user_id: user_id,
                message: message.val(),
                time: Date.now(),
                game_id: Session.get("current_game")
                });
        Aparte del id del usuario y el mensaje guardamos en la base de datos la hora de la publicación del mensaje y el juego en el que se haya escrito el mensaje (char general, alien invasion, frootswar). Se vuelve a poner el valor de message a '' message.val('') 

4. Se termina partida con puntuación alta sin estar autenticado.
        Si se termina una partida sin estar autenticado las siguientes líneas no se ejecutan por tanto, no  aparece nada.
        matchFinish: function (game, points) {
	if (this.userId)
	    Matches.insert ({user_id: this.userId, 
			     time_end: Date.now(),
			     points: points,
			     game_id: game
			    });
        }

5. Se termina partida con puntuación alta estando autenticado.

        Si se termina la partida estando autenticado se ejecutan las siguientes líneas:
        if (this.userId)
	    Matches.insert ({user_id: this.userId, 
			     time_end: Date.now(),
			     points: points,
			     game_id: game
			    });
        }
        Se guarda en la base de datos el id del usuario, la hora de finalización de la partida, la puntuación y el id del juego, si esa puntuación es alta se actualiza el template de las puntuaciones y aparece. líneas 157-171.

6. ¿Qué sale en la consola cuando te autenticas (sing in)?
        [12:44:06.635] "current user: null"
        [12:44:06.636] "current user: 7vfZXe65JQ9gBxx7o"
        Deps.autorun(function(){
                console.log("current user: " + currentUser);
                currentUser = Meteor.userId();
                console.log("current user: " + currentUser);
        });
        Esas son las líneas que se ejecutan al inciar sesión. con currentUser = Meteor.userId(); se coje el id del usuario con el que estoy inciando sesión que es el que sale en la segunda línea de la consola.

7. ¿Qué sale en la consola cuando te sales (sing out)?
        [13:07:18.904] "current user: 7vfZXe65JQ9gBxx7o"
        [13:07:18.904] "current user: null"
        El usuario está guardado y cuando sales se pone current user a null.

