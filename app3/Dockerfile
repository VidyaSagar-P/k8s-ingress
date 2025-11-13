FROM nginx
RUN rm -rf /usr/share/nginx/html/index.html
RUN printf '%s\n' '<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Love Note</title>
  <style>
    body {
      margin:0;
      height:100vh;
      display:flex;
      justify-content:center;
      align-items:center;
      text-align:center;
      background:linear-gradient(135deg,#ffdde1,#ee9ca7);
      font-family:sans-serif;
      color:white;
    }
    h1 { font-size:2em; line-height:1.6; }
  </style>
</head>
<body>
  <h1>Hey Angel,<br><br>
  Just a small note to say — I love you more every single day.<br><br>
  You’re my world, my peace, my forever.<br><br>
  — From your Banti Bava
  </h1>
</body>
</html>' > /usr/share/nginx/html/index.html

