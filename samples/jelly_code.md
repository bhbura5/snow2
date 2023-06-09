
```html

<?xml version="1.0" encoding="utf-8" ?>
<j:jelly trim="false" xmlns:j="jelly:core" xmlns:g="glide" xmlns:j2="null" xmlns:g2="null">

	
	<!-- NOTE the tag attributes VALUES must be wraped in a string: foo='bar' OR foo='${"bar"}' -->
	<!-- JELLY set tag, Attributes "var,value" -->
	<j:set var='jvar_somevar' value='100'/>
	
	<!-- This is a comment -->
	<p>This is html ${" and JEXL"} AND $[" SN JEXL!"]</p>
	<p>Loop  = ${jvar_somevar} </p>
	<p>Loop(does not work!) = $[jvar_somevar] </p>
	<br />
	
	<j:set var='jvar_loop' value='0'/>
	<!-- JELLY while tag, Attributes "test" -->
	<j:while test='${jvar_loop lt 3}'>
		<p>${jvar_loop}</p>
		<!-- Note: set tag ALWAYS requires var and value attributes -->
		<!-- Doublecheck your varable names in a LOOP! -->
		<j:set var='jvar_loop' value='${jvar_loop + 1}' />
	</j:while>
	
	
	<!-- $[SP] == html non-breaking space '&nbsp;' -->
	
	<!-- Conditiona tags: if, choose, case -->
	<j:set var='jvar_mytest1' value='true' />
	
	<j:if test='${jvar_mytest1}' >
		<p>I'm true</p>
	</j:if>
	
	<j:if test='${!jvar_mytest1}' >
		<!-- I'm not rendered ! -->
		<p>I'm false!</p>
		
	</j:if>
	
	<!-- build an array in Jelly-->
	<g:evaluate> <!-- ServiceNow Jelly Extension -->
		var colors = ['Red', 'Black', 'Blue','Brown','Yellow','Green'];
	</g:evaluate>
	
	<!-- Iterate over over a COLLECTION -->
	<j:forEach var='jvar_color' items='${colors}'  >
		<p>Color: $[SP] <span style="color:${jvar_color};">${jvar_color}</span></p>
		
	</j:forEach>
	
	<!-- build an array of OBJECTS in Jelly -->
	<g:evaluate>
		var colors2 = [
		{name: 'red', value: '#FF0000'},
		{name: 'blue', value: '#0000FF'}
		];
	</g:evaluate>
	<j:forEach var='jvar_color2' items='${colors2}'>
		<!-- notes the 'jelly="true" attribute. There is also a 'object="true" attribute -->
		<g:evaluate jelly='true'>
			var name = jelly.jvar_color2.name;
			var value = jelly.jvar_color2.value;
		</g:evaluate>
		<p>Color: $[SP] <span style="color:${value};">${name}</span></p>
	</j:forEach>
	
	<!-- g:evaluate tag executes JavaScript on the ServiceNow instance -->
	
	<g:evaluate>
		gs.info("CM*** From Jelly!");
		var auxdb = gs.getProperty('auxdb.db.url');
	</g:evaluate>
	
	<p>Auxdb = $[SP] ${auxdb}</p>
	
	<!-- GLIDERECORD in JELLY -->
	<g:evaluate>
		var user = gs.getUser();
		var gr = new GlideRecord('sys_user_has_role');
		gr.addQuery('user', user.getID());
		gr.query();
		var roleIDs = [];
		while(gr.next()){
		roleIDs.push('' + gr.role);
		//gs.info("CM*** found: " + gr.role);
		}
		gr = new GlideRecord('sys_user_role');
		gr.addQuery('sys_id', roleIDs);
		gr.query();
		
	</g:evaluate>
	
	<p>Hello ${user.getFirstName()} $[SP] ${user.getID()}</p>
	<p>You have the following roles:</p>
	<j:while test='${gr.next()}'> <!-- gr.next() to controle while -->
		<p style="margin-left:15px">${gr.name}</p>
	</j:while>
	
	<!-- Assign the result of an evaluate to a JELLY varable -->
	<g:evaluate var='jvar_cssSplit'>
		//Last expression evaluated will be assigned to jvar_cssSplit
		gs.getProperty('css.form.vsplit');
	</g:evaluate>
	<p>CSS Form VSPLIT: $[SP] ${jvar_cssSplit}</p>
	
	<!-- Note you can enter "reserved" XML charters like '&" so use ${AMP} or '&amp;amp;' -->
	<!-- Note never use JEXL in an evaluate tag, unless it ALWAYS resolves to the same thing: ${AMP} -->
	
	<!-- evaluate jelly='true' allow JELLY varables to be 'visiable' in the evaluate tag via 'jelly.foo' pattren -->
	<j:set var='jvar_inEval' value='Hello from a JELLY Varable!' />
	<g:evaluate jelly='true'>
		var fooBar = jelly.jvar_inEval;
	</g:evaluate>
	<p>Just getting a JELLY varable in evaluate: $[SP] ${fooBar}</p>
	
	<!-- Note:the evaluate tag will return a string to "var=" by default -->
	<!-- Note: use object='true' to return an object(Java collection) in evaluate tag to use in forEach-->
	<!-- Jelly Phase I cached per user: "Template", Jelly Phase II dynamic -->
	<!-- j,g Phase I, j2, g2 Phase II -->
	<!-- ${} Phase I, $[] Phase II -->
	
	<!-- Variables -->
	<!-- JELLY only knows about JELLY variables: jvar_foo -->
	<!-- JavaScript can get a R/O COPY of JELLY variables via jelly='true' in evaluate tag-->
	<!-- JEXL assumes JELLY if varable starts with "jvar_" ELSE JavaScript variable -->
	
	<!-- Pass JavaScript Var to JELLY -->
	<g:evaluate var="jvar_fromJS">
		var foo = "I'm a JS Variable";
		foo;
	</g:evaluate>
	<p>JS to JELLY: $[SP] ${jvar_fromJS}</p>
	
	<!-- Pass JELLY to JavaScript-->
	<j:set var="jvar_jellyToJS" value="I'm a Jelly Variable"/>
	<g:evaluate jelly="true">
		var jellyToJS = jelly.jvar_jellyToJS;
	</g:evaluate>
	<p>JELLY to JS: $[SP] ${jellyToJS}</p>
		
	<!-- Watch for Phase Miss Match between tags and JEXL -->
	<!-- Phase I: j,g use ${}, Phase II: j2,g2 use $[] -->
	
	<!-- Copy Phase I var to Phase II -->
	<g:evaluate var="jvar_phaseI_phaseII" copyToPhase2="true">
   var specialInc = "I started in Phase I";
   specialInc;
	</g:evaluate>
	<p>Run in Phase II: $[SP] $[jvar_phaseI_phaseII] </p>

	<br />
	<p></p>
	<!-- Server Side JS to Client Side JS -->
	<g2:evaluate>
               var y = Math.random() * 100;
               y;
       </g2:evaluate>
       <g:evaluate>
             var  color = 'Green';
       </g:evaluate>
     

            Hello, $[SP]
	<a id='ack'> $[gs.getUser().getFirstName()] </a>.
	
   
 
	<script>
               $('ack').onclick = function(event){
                       var e = $('ack');
                       e.style.color = '${color}';
						alert("Click");
               }
	</script>
	
     
       <g2:evaluate/>
	
	<!-- -->
	<br />
	<input id="name" type="text" />
	<input id="doit" type="submit" />
	<script>
		$('doit').onclick = function(event){
			window.location =  'jelly.do?sysparm_submited=true${AMP}sysparm_name=' +
			$('name').value;
		}
	</script>
	
	<g2:evaluate jelly='true'>
	var incomming = (jelly.sysparm_submited != null);

	</g2:evaluate>
	<p>Return: $[SP] $[sysparm_submited] $[SP] * $[incomming] *</p>
	<j2:if test='$[sysparm_submited]'>
		<p>Hello: $[SP] $[sysparm_name]</p>
	</j2:if >
	
	

	
	
</j:jelly>