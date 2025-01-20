client-side validation _should not be considered_ an exhaustive security measure! Your apps should always perform security checks on any form-submitted data on the _server-side_ **as well** as the client-side, because client-side validation is too easy to bypass, so malicious users can still easily send bad data through to your server.
## Using built-in form validation
One of the most significant features of modern form controls is the ability to validate most user data without relying on JavaScript. This is done by using validation attributes on form elements. We've seen many of these earlier in the course, but to recap:
- [`required`](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/required): Specifies whether a form field needs to be filled in before the form can be submitted.
- [`minlength`](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/minlength) and [`maxlength`](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/maxlength): Specifies the minimum and maximum length of textual data (strings).
- [`min`](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/min) and [`max`](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/max): Specifies the minimum and maximum values of numerical input types.
- [`type`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input#input_types): Specifies whether the data needs to be a number, an email address, or some other specific preset type.
- [`pattern`](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/pattern): Specifies a [regular expression](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_expressions) that defines a pattern the entered data needs to follow.

If the data entered in a form field follows all of the rules specified by the above attributes, it is considered valid. If not, it is considered invalid.
When an element is valid, the following things are true:
- The element matches the [`:valid`](https://developer.mozilla.org/en-US/docs/Web/CSS/:valid) CSS pseudo-class, which lets you apply a specific style to valid elements.
When an element is invalid, the following things are true:
- The element matches the [`:invalid`](https://developer.mozilla.org/en-US/docs/Web/CSS/:invalid) CSS pseudo-class, and sometimes other UI pseudo-classes (e.g., [`:out-of-range`](https://developer.mozilla.org/en-US/docs/Web/CSS/:out-of-range)) depending on the error, which lets you apply a specific style to invalid elements.

For example- 
```HTML
<form>
  <label for="choose">Would you prefer a banana or a cherry? (required)</label>  //required attribute
  <input id="choose" name="i-like" required pattern="[Bb]anana|[Cc]herry" />  //rewuired pattern matches that and does stuff
  <button>Submit</button>
</form>
```
#### Constraining the length of your entries
You can constrain the character length of all text fields created by [`<input>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input) or [`<textarea>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/textarea) by using the [`minlength`](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/minlength) and [`maxlength`](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/maxlength) attributes. A field is invalid if it has a value and that value has fewer characters than the [`minlength`](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/minlength) value or more than the [`maxlength`](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/maxlength) value.
Browsers often don't let the user type a longer value than expected into text fields. A better user experience than just using `maxlength` is to also provide character count feedback in an accessible manner and let them edit their content down to size. An example of this is the character limit when posting on social media. JavaScript, including [solutions using `maxlength`](https://github.com/mimo84/bootstrap-maxlength), can be used to provide this.

# Using JavaScript - 
#### Implementing a customized error message
As you saw in the HTML validation constraint examples earlier, each time a user tries to submit an invalid form, the browser displays an error message. The way this message is displayed depends on the browser.

These automated messages have two drawbacks:
- There is no standard way to change their look and feel with CSS.
- They depend on the browser locale, which means that you can have a page in one language but an error message displayed in another language, as seen in the following Firefox screenshot.
Customizing these error messages is one of the most common use cases of the Constraint Validation API. Let's work through a simple example of how to do this.
```
<form>
  <label for="mail">
    I would like you to provide me with an email address:
  </label>
  <input type="email" id="mail" name="mail" />
  <button>Submit</button>
</form>
```
And add the following JavaScript to the page:
```
const email = document.getElementById("mail");

email.addEventListener("input", (event) => {
  if (email.validity.typeMismatch) {
    email.setCustomValidity("I am expecting an email address!");
  } else {
    email.setCustomValidity("");
  }
});
```

Here we store a reference to the email input, then add an event listener to it that runs the contained code each time the value inside the input is changed.

Inside the contained code, we check whether the email input's `validity.typeMismatch` property returns `true`, meaning that the contained value doesn't match the pattern for a well-formed email address. If so, we call the [`setCustomValidity()`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLInputElement/setCustomValidity "setCustomValidity()") method with a custom message. This renders the input invalid, so that when you try to submit the form, submission fails and the custom error message is displayed.
If the `validity.typeMismatch` property returns `false`, we call the `setCustomValidity()` method with an empty string. This renders the input valid, so the form will submit.

**Fully overriding browser validation -**
```HTML
<form novalidate>
  <p>
    <label for="mail">
      <span>Please enter an email address:</span>
      <input type="email" id="mail" name="mail" required minlength="8" />
      <span class="error" aria-live="polite"></span>
    </label>
  </p>
  <button>Submit</button>
</form>
```
```JavaScript
// There are many ways to pick a DOM node; here we get the form itself and the email
// input box, as well as the span element into which we will place the error message.
const form = document.querySelector("form");
const email = document.getElementById("mail");
const emailError = document.querySelector("#mail + span.error");

email.addEventListener("input", (event) => {
  // Each time the user types something, we check if the
  // form fields are valid.

  if (email.validity.valid) {
    // In case there is an error message visible, if the field
    // is valid, we remove the error message.
    emailError.textContent = ""; // Reset the content of the message
    emailError.className = "error"; // Reset the visual state of the message
  } else {
    // If there is still an error, show the correct error
    showError();
  }
});

form.addEventListener("submit", (event) => {
  // if the email field is valid, we let the form submit
  if (!email.validity.valid) {
    // If it isn't, we display an appropriate error message
    showError();
    // Then we prevent the form from being sent by canceling the event
    event.preventDefault();
  }
});

function showError() {
  if (email.validity.valueMissing) {
    // If the field is empty,
    // display the following error message.
    emailError.textContent = "You need to enter an email address.";
  } else if (email.validity.typeMismatch) {
    // If the field doesn't contain an email address,
    // display the following error message.
    emailError.textContent = "Entered value needs to be an email address.";
  } else if (email.validity.tooShort) {
    // If the data is too short,
    // display the following error message.
    emailError.textContent = `Email should be at least ${email.minLength} characters; you entered ${email.value.length}.`;
  }

  // Set the styling appropriately
  emailError.className = "error active";
}

```
[[Async Functions -]]