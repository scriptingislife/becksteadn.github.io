---
layout: page
title: Contact Me
permalink: /contact-me/
published: true
---

<form action="https://formspree.io/blog@mail.scriptingis.life" method="POST">

  <header>
    <div>Request a resume or just ask a question!</div>
    <style>
 * {
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
}

body {
  padding: 20px 15%;
}
form header {
  margin: 0 0 20px 0; 
}
form header div {
  font-size: 90%;
  color: #999;
}
form header h2 {
  margin: 0 0 5px 0;
}
form > div {
  clear: both;
  overflow: hidden;
  padding: 1px;
  margin: 0 0 10px 0;
}
form > div > fieldset > div > div {
  margin: 0 0 5px 0;
}
form > div > label,
legend {
	width: 25%;
  float: left;
  padding-right: 10px;
}
form > div > div,
form > div > fieldset > div {
  width: 75%;
  float: right;
}
form > div > fieldset label {
	font-size: 90%;
}
fieldset {
	border: 0;
  padding: 0;
}

input[type=text],
input[type=email],
input[type=url],
input[type=password],
textarea {
	width: 100%;
  border-top: 1px solid #ccc;
  border-left: 1px solid #ccc;
  border-right: 1px solid #eee;
  border-bottom: 1px solid #eee;
}
input[type=text],
input[type=email],
input[type=url],
input[type=password] {
  width: 50%;
}
input[type=text]:focus,
input[type=email]:focus,
input[type=url]:focus,
input[type=password]:focus,
textarea:focus {
  outline: 0;
  border-color: #4697e4;
}

@media (max-width: 600px) {
  form > div {
    margin: 0 0 15px 0; 
  }
  form > div > label,
  legend {
	  width: 100%;
    float: none;
    margin: 0 0 5px 0;
  }
  form > div > div,
  form > div > fieldset > div {
    width: 100%;
    float: none;
  }
  input[type=text],
  input[type=email],
  input[type=url],
  input[type=password],
  textarea,
  select {
    width: 100%; 
  }
}
@media (min-width: 1200px) {
  form > div > label,
	legend {
  	text-align: right;
  }
}
    </style>
  </header>
  
  <div>
    <label class="desc" id="name">Full Name</label>
    <div>
      <input id="Name" name="Name" type="text" class="field text fn" placeholder="Name" size="8" tabindex="1">
    </div>
  </div>
    
    <div>
    <label class="desc" id="company">Company</label>
    <div>
      <input id="Company" name="Company" type="text" class="field text fn" placeholder="Company" size="8" tabindex="1">
    </div>
  </div>
    
  <div>
    <label class="desc" id="email">
      Email
    </label>
    <div>
      <input id="Email" name="Email" type="email" spellcheck="false" placeholder="Email" maxlength="255" tabindex="3"> 
   </div>
  </div>
    
  <div>
    <label class="desc" id="comments">
      Comments
    </label>
  
    <div>
      <textarea id="Comments" name="Comments" spellcheck="true" rows="10" cols="50" tabindex="4" ></textarea>
    </div>
  </div>
    
  <div>
      <div>
  		<input id="saveForm" type="submit" value="Submit">
    </div>
	</div>
  
</form>

<a href="https://codepen.io/chriscoyier/pen/DmnlJ">Responsive Form By Chris Coyler</a>
