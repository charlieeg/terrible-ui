<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Create Account</title>
  <style>
    body {
      font-family: 'Arial', sans-serif;
      background-color: #f4f4f9;
      margin: 0;
      padding: 20px;
    }

    .signup-container {
      max-width: 450px;
      margin: 50px auto;
      background: #ffffff;
      padding: 30px;
      border-radius: 10px;
      box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
    }

    h1 {
      text-align: center;
      margin-bottom: 20px;
      font-size: 24px;
      color: #333333;
    }

    label {
      font-size: 14px;
      font-weight: bold;
      display: block;
      margin-bottom: 5px;
      color: #555555;
    }

    input[type="text"], input[type="password"], textarea {
      width: 100%;
      padding: 10px;
      margin-bottom: 10px;
      border: 1px solid #cccccc;
      border-radius: 5px;
      font-size: 14px;
    }

    textarea {
      resize: none;
    }

    .email-domain {
      margin-bottom: 20px;
    }

    .email-domain-options {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
    }

    .radio-group {
      display: flex;
      align-items: center;
      gap: 5px;
    }

    .radio-group span {
      font-size: 14px;
      color: #555;
    }

    .other-email {
      display: none;
      margin-top: 10px;
    }

    .gender-container {
      display: flex;
      flex-wrap: wrap;
      gap: 15px;
      margin-bottom: 20px;
    }

    .other-gender {
      display: none;
      margin-top: 10px;
    }

    button {
      width: 100%;
      padding: 12px;
      font-size: 16px;
      color: #ffffff;
      background-color: #007bff;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      transition: 0.3s;
    }

    button:hover {
      background-color: #0056b3;
    }

    .slider-container {
      margin-bottom: 20px;
    }

    .slider-container input[type="range"] {
      width: 100%;
      margin: 10px 0;
    }

    .slider-output {
      font-size: 14px;
      font-weight: bold;
      color: #007bff;
    }

    .password-error {
      color: #e74c3c;
      font-size: 12px;
      margin-top: 5px;
    }

    .password-hint {
      font-size: 12px;
      color: #555;
      margin-top: 5px;
    }
  </style>
</head>
<body>
  <div class="signup-container">
    <h1>Create Account</h1>
    <form id="signup-form" onsubmit="return validateForm(event)">
      <!-- Full Name -->
      <label for="name">Full Name</label>
      <input type="text" id="name" placeholder="Enter your full name" onfocus="clearPlaceholder(this)" onblur="restorePlaceholder(this)">

      <!-- Email -->
      <label for="email">Email</label>
      <input type="text" id="email" placeholder="Enter your email username (e.g., johndoe)" onfocus="clearPlaceholder(this)" onblur="restorePlaceholder(this)">
      <div class="email-domain">
        <label>Choose your email domain:</label>
        <div class="email-domain-options">
          <div class="radio-group">
            <input type="radio" name="email-part" value="gmail.com" onchange="toggleOtherEmail(false)"> <span>@gmail.com</span>
          </div>
          <div class="radio-group">
            <input type="radio" name="email-part" value="yahoo.com" onchange="toggleOtherEmail(false)"> <span>@yahoo.com</span>
          </div>
          <div class="radio-group">
            <input type="radio" name="email-part" value="hotmail.com" onchange="toggleOtherEmail(false)"> <span>@hotmail.com</span>
          </div>
          <div class="radio-group">
            <input type="radio" name="email-part" value="other" onchange="toggleOtherEmail(true)"> <span>Other</span>
          </div>
        </div>
        <div class="other-email">
          <label for="other-email-domain">Enter your email domain (e.g., mydomain.net):</label>
          <input type="text" id="other-email-domain" placeholder="e.g., mydomain.net">
        </div>
      </div>

      <!-- Password -->
      <label for="password">Password</label>
      <input type="password" id="password" placeholder="Create a password" onfocus="clearPasswordHint()" onblur="restorePasswordHint()">
      <div id="password-error" class="password-error"></div>
      <div id="password-hint" class="password-hint">Password must be between 12-14 characters, include the letter 'q', and contain a prime number.</div>

      <!-- Gender -->
      <label>Gender</label>
      <div class="gender-container">
        <div class="radio-group">
          <input type="radio" name="gender" value="male" onchange="toggleOtherGender(false)"> Male
        </div>
        <div class="radio-group">
          <input type="radio" name="gender" value="female" onchange="toggleOtherGender(false)"> Female
        </div>
        <div class="radio-group">
          <input type="radio" name="gender" value="other" onchange="toggleOtherGender(true)"> Other
        </div>
      </div>
      <div class="other-gender">
        <label for="other-gender-text">Please describe your gender in 100 characters or less:</label>
        <textarea id="other-gender-text" rows="3" maxlength="100" placeholder="Describe here..."></textarea>
      </div>

      <!-- Date of Birth -->
      <label>Date of Birth</label>
      <div class="slider-container">
        <small>(Slide to pick your exact date of birth)</small>
        <input 
          type="range" 
          id="dob-slider" 
          min="0" 
          max="45144" 
          value="20000" 
          oninput="updateDOBSlider(this.value)">
        <div class="slider-output" id="dob-output">1990-06-15</div>
      </div>

      <!-- Submit -->
      <button type="submit">Sign Up</button>
    </form>
  </div>

  <script>
    // Update the combined slider output for Date of Birth
    function updateDOBSlider(value) {
      const startYear = 1900; // Minimum year
      const daysInYear = 365; // Approximate days in a year
      const year = startYear + Math.floor(value / daysInYear);
      const month = Math.floor((value % daysInYear) / 30) + 1;
      const day = (value % 30) + 1;

      const formattedMonth = month < 10 ? `0${month}` : month;
      const formattedDay = day < 10 ? `0${day}` : day;

      document.getElementById("dob-output").textContent = `${year}-${formattedMonth}-${formattedDay}`;
    }

    function toggleOtherEmail(show) {
      document.querySelector('.other-email').style.display = show ? 'block' : 'none';
    }

    function toggleOtherGender(show) {
      document.querySelector('.other-gender').style.display = show ? 'block' : 'none';
    }

    // Helper function to check if a number is prime
    function isPrime(num) {
      if (num <= 1) return false;
      for (let i = 2; i < num; i++) {
        if (num % i === 0) return false;
      }
      return true;
    }

    // Placeholder functions to make the user delete placeholder text manually
    function clearPlaceholder(input) {
      if (input.value === input.placeholder) {
        input.value = '';
      }
    }

    function restorePlaceholder(input) {
      if (!input.value) {
        input.value = input.placeholder;
      }
    }

    // Password hint functions
    function clearPasswordHint() {
      document.getElementById('password-hint').style.display = 'none';
    }

    function restorePasswordHint() {
      if (!document.getElementById('password').value) {
        document.getElementById('password-hint').style.display = 'block';
      }
    }

    // Form validation
    function validateForm(event) {
      event.preventDefault();

      const name = document.getElementById('name').value;
      const email = document.getElementById('email').value;
      const emailPart = document.querySelector('input[name="email-part"]:checked');
      const otherEmailDomain = document.getElementById('other-email-domain').value;
      const password = document.getElementById('password').value;
      const gender = document.querySelector('input[name="gender"]:checked');
      const otherGenderText = document.getElementById('other-gender-text').value;

      // Full name validation
      if (!name || name === 'Enter your full name') {
        alert("Please enter your full name!");
        return false;
      }

      // Email validation
      if (!email || email === 'Enter your email username (e.g., johndoe)') {
        alert("Please enter your email username!");
        return false;
      }
      if (!emailPart) {
        alert("Please select an email domain!");
        return false;
      }
      if (emailPart.value === "other" && !otherEmailDomain) {
        alert("Please enter a valid email domain!");
        return false;
      }

      // Password validation
      const errorMessageDiv = document.getElementById('password-error');
      errorMessageDiv.textContent = ''; // Clear previous error messages

      if (!password || password.length < 12 || password.length > 14) {
        errorMessageDiv.textContent = "Password must be between 12-14 characters!";
        return false;
      }

      if (!password.includes('q')) {
        errorMessageDiv.textContent = "Password must include the letter 'q'!";
        return false;
      }

      // Check for a prime number in the password
      let hasPrime = false;
      for (let i = 0; i < password.length; i++) {
        if (!isNaN(password[i]) && isPrime(Number(password[i]))) {
          hasPrime = true;
          break;
        }
      }

      if (!hasPrime) {
        errorMessageDiv.textContent = "Password must include at least one prime number";
        return false;
      }

      // Gender validation
      if (!gender) {
        alert("Please select your gender");
        return false;
      }
      if (gender.value === "other" && otherGenderText.length > 100) {
        alert("Your gender description is too long! Keep it under 100 characters.");
        return false;
      }

      alert("Sign-up successful! Thanks for struggling with this form!");
    }
  </script>
</body>
</html>
