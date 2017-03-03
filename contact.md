---
layout: default
title: Contact Obaida
---

<div id="contact">
  <h1 class="pageTitle">Contact Me</h1>
  <div class="contactContent">
    <p class="intro">Feel free to shoot me an email. Email me:  <a href="mailto:obaida007@gmail.com">click here</a>. Thanks!</p>
    <p> If you want to make changes then do so in the <code>contact.html</code> file.  The form is provided by <a href="http://formspree.io/">Formspree.</a> Follow the directions on their site to set up the form for use.</p>
  </div>
  <form action="http://formspree.io/your@mail.com" method="POST">
    <label for="name">Name</label>    
    <input type="text" id="name" name="name" class="full-width"><br>
    <label for="email">Email Address</label>
    <input type="email" id="email" name="_replyto" class="full-width"><br>
    <label for="message">Message</label>
    <textarea name="message" id="message" cols="30" rows="10" class="full-width"></textarea><br>
    <input type="submit" value="Send" class="button">
  </form>
</div>
