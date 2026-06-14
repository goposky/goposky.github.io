---
layout: page
title: About
permalink: /about/
---

Hi,

This is the personal blog site of Gopal Ramachandran.

Find me on [GitHub](https://github.com/goposky), [LinkedIn](https://www.linkedin.com/in/goposky), [Speakerdeck](https://speakerdeck.com/goposky), or [Twitter](https://twitter.com/goposky).

Or drop me a message below.

<form id="contact-form" class="contact-form" action="https://api.web3forms.com/submit" method="POST">
  <input type="hidden" name="access_key" value="{{ site.web3forms_access_key }}">
  <input type="hidden" name="subject" value="New message from {{ site.title }}">
  <input type="hidden" name="from_name" value="{{ site.title }}">
  <input type="checkbox" name="botcheck" class="hidden" style="display:none;" tabindex="-1" autocomplete="off">

  <div class="form-field">
    <label for="name">Name</label>
    <input type="text" id="name" name="name" required>
  </div>

  <div class="form-field">
    <label for="email">Email</label>
    <input type="email" id="email" name="email" required>
  </div>

  <div class="form-field">
    <label for="message">Message</label>
    <textarea id="message" name="message" rows="6" required></textarea>
  </div>

  <button type="submit" class="form-submit">Send message</button>
  <p id="form-result" class="form-result" role="status" aria-live="polite"></p>
</form>

<script>
  (function () {
    var form = document.getElementById('contact-form');
    var result = document.getElementById('form-result');
    form.addEventListener('submit', function (e) {
      e.preventDefault();
      var data = new FormData(form);
      result.className = 'form-result';
      result.textContent = 'Sending…';
      fetch('https://api.web3forms.com/submit', { method: 'POST', body: data })
        .then(function (res) { return res.json().then(function (json) { return { ok: res.ok, json: json }; }); })
        .then(function (r) {
          if (r.ok) {
            result.classList.add('success');
            result.textContent = r.json.message || 'Thanks! Your message has been sent.';
            form.reset();
          } else {
            result.classList.add('error');
            result.textContent = r.json.message || 'Something went wrong. Please try again.';
          }
        })
        .catch(function () {
          result.classList.add('error');
          result.textContent = 'Something went wrong. Please try again.';
        });
    });
  })();
</script>
