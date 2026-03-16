# Fc-lion-star
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>FC Lions Star</title>

<!-- Google Fonts -->
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">

<style>
/* General Styles */
body {
  margin: 0;
  font-family: 'Roboto', sans-serif;
  background-color: #0b6623;
  color: white;
  text-align: center;
}

/* Header and Logo */
header {
  background-color: #111;
  padding: 20px;
}
header img {
  width: 180px;
  border-radius: 50%;
  border: 3px solid white;
}
header h1 {
  margin: 10px 0 0 0;
  font-size: 2.5em;
  letter-spacing: 2px;
}

/* About Section */
section {
  background: white;
  color: black;
  margin: 20px auto;
  padding: 20px;
  border-radius: 10px;
  width: 90%;
  max-width: 800px;
}
section h2 {
  color: #0b6623;
}

/* Matches */
ul {
  list-style: none;
  padding: 0;
}
ul li {
  padding: 10px 0;
  border-bottom: 1px solid #ccc;
}

/* Gallery */
.gallery {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 15px;
  margin: 20px 0;
}
.gallery img {
  width: 200px;
  height: 120px;
  object-fit: cover;
  border-radius: 8px;
  border: 2px solid white;
}

/* Contact */
.contact p {
  margin: 5px 0;
  font-size: 1.1em;
}

/* Footer */
footer {
  margin-top: 20px;
  padding: 10px;
  background-color: #111;
  color: #ccc;
  font-size: 0.9em;
}

/* Buttons */
.button {
  display: inline-block;
  padding: 10px 20px;
  margin: 15px 0;
  background-color: #111;
  color: white;
  text-decoration: none;
  border-radius: 5px;
  transition: 0.3s;
}
.button:hover {
  background-color: #444;
}
</style>
</head>

<body>

<header>
  <img src="lionfc-logo.jpg" alt="FC Lions Star Logo">
  <h1>⚽ FC Lions Star</h1>
</header>

<section>
  <h2>About Our Club</h2>
  <p>Welcome to FC Lions Star! We are dedicated to football excellence. Follow our updates, matches, and club news right here.</p>
</section>

<section>
  <h2>Latest Matches</h2>
  <ul>
    <li>FC Lions Star 2 - 1 Young Stars</li>
    <li>FC Lions Star 3 - 0 City Boys</li>
    <li>FC Lions Star 1 - 1 Eagles United</li>
  </ul>
</section>

<section>
  <h2>Match Gallery</h2>
  <div class="gallery">
    <img src="match1.jpg" alt="Match 1">
    <img src="match2.jpg" alt="Match 2">
    <img src="match3.jpg" alt="Match 3">
  </div>
</section>

<section class="contact">
  <h2>Contact Us</h2>
  <p>WhatsApp: 09115568667</p>
  <p>Instagram: Fc_lions_star</p>
  <a href="https://wa.me/09115568667" class="button">Chat on WhatsApp</a>
</section>

<footer>
  &copy; 2026 FC Lions Star. All rights reserved.
</footer>

</body>
</html>
