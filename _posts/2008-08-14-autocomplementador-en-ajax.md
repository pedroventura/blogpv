---
layout: post
title: "Autocomplementador en AJAX"
date: 2008-08-14 00:00:00 +0000
categories: ["Javascript"]
tags: ["jquery", "tutoriales"]
---

<p ><strong><em>Como hacer un Autocomplementador simple en AJAX</em></strong></p>
<p ><a href="https://www.pedroventura.com/wp-content/uploads/2008/08/autocomplete_2.png"><img class="alignnone size-medium wp-image-16 alignright" style="float: right;" title="autocomplete_2" src="https://pedroventura.com/wp-content/uploads/2008/08/autocomplete_2-252x3001-1-1.png" alt="" width="252" height="300" /></a></p>
Pasos para la implementación de éste módulo:
<ul>
	<li>incluir el siguiente .js entre las etiquetas &lt;head&gt;&lt;/head&gt;</li>
</ul>
Ver .js

y las funciones javascript que podréis ver en la <strong>Demo Online </strong>
<ul>
	<li>Incluir también los estilos predeterminados, también los podréis ver en el código fuente de éste archivo.</li>
</ul>
<ul>
	<li> En la carpeta donde se encuentra éste ejemplo hay un php "rpc.php" que contiene la llamada a la base de datos y el cual devuelve el contenido.</li>
</ul>
[code lang="php"]
    &amp;lt;?php

    // PHP5 Implementation - uses MySQLi.
    // mysqli('localhost', 'yourUsername', 'yourPassword', 'yourDatabase');
    $db = new mysqli('localhost', 'USERNAME' ,'PASSWORD', 'DATABASE');

    if(!$db) {
    // Show error if we cannot connect.
    echo 'ERROR: Could not connect to the database.';
    } else {
    // Is there a posted query string?
    if(isset($_POST['queryString'])) {
    $queryString = $db-&amp;gt;real_escape_string($_POST['queryString']);

    // Is the string length greater than 0?

    if(strlen($queryString) &amp;gt;0) {
    // Run the query: We use LIKE '$queryString%'
    // The percentage sign is a wild-card, in my example of countries it works like this...
    // $queryString = 'Uni';
    // Returned data = 'United States, United Kindom';

    // YOU NEED TO ALTER THE QUERY TO MATCH YOUR DATABASE.
    // eg: SELECT yourColumnName FROM yourTable WHERE yourColumnName LIKE '$queryString%' LIMIT 10

    $query = $db-&amp;gt;query(&amp;quot;SELECT your_column FROM your_db_table WHERE your_column LIKE '$queryString%' LIMIT 10&amp;quot;);
    if($query) {
    // While there are results loop through them - fetching an Object (i like PHP5 btw!).
    while ($result = $query -&amp;gt;fetch_object()) {
    // Format the results, im using &amp;lt;li&amp;gt; for the list, you can change it.
    // The onClick function fills the textbox with the result.

    // YOU MUST CHANGE: $result-&amp;gt;value to $result-&amp;gt;your_colum
    echo '&amp;lt;li onClick=&amp;quot;fill(''.$result-&amp;gt;value.'');&amp;quot;&amp;gt;'.$result-&amp;gt;value.'&amp;lt;/li&amp;gt;';
    }
    } else {
    echo 'ERROR: There was a problem with the query.';
    }
    } else {
    // Dont do anything.
    } // There is a queryString.
    } else {
    echo 'There should be no direct access to this script!';
    }
    }
    ?&amp;gt;
[/code]

Si usais SMARTY, habría que poner incluso menos código:
[code lang="php"]
    &amp;lt;?
    include(&amp;quot;/ruta_smarty_donde_teneis/config.php&amp;quot;);
    include_once(&amp;quot;/ruta_smarty_donde_teneis/sql.lib.php&amp;quot;);
    abrirConexion();// o la funcion que hayas nombrado para crear una conexión
    $busqueda= $_POST['queryString']; // $_POST['queryString'] es la variable que lleva lo que estamos insertando en el formulario de búsqueda
    $sql_search=&amp;quot;SELECT NOMBRE_CAMPO FROM NOMBRE_TABLA WHERE NOMBRE_CAMPO LIKE '$busqueda%' LIMIT 10&amp;quot;;
    $db-&amp;gt;query($sql_search);
    //$searching = $db-&amp;gt;fetchObject();
    while ($searching = $db -&amp;gt;fetchObject())
    {
    echo '&amp;lt;li onClick=&amp;quot;fill(''.$searching-&amp;gt;NombreFamoso.'');&amp;quot;&amp;gt;'.$searching-&amp;gt;NombreFamoso.'&amp;lt;/li&amp;gt;';
    }

    ?&amp;gt;
[/code]
