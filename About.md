---
permalink: /about/
title: About
---
<html>
<head>
<style>
div.gallery {
  border: 1px solid #ccc;
}

div.gallery:hover {
  border: 1px solid #777;
}

div.gallery img {
  width: 100%;
  height: auto;
}

div.desc {
  padding: 15px;
  text-align: center;
}

* {
  box-sizing: border-box;
}

.responsive {
  padding: 0 6px;
  float: left;
  width: 24.99999%;
}

@media only screen and (max-width: 700px) {
  .responsive {
    width: 49.99999%;
    margin: 6px 0;
  }
}

@media only screen and (max-width: 500px) {
  .responsive {
    width: 100%;
  }
}

.clearfix:after {
  content: "";
  display: table;
  clear: both;
}
</style>
</head>
<body>

<h2>About</h2>

<h4>About page that links to our personal projects.</h4>

<div class="responsive">
  <div class="gallery">
    <a target="_blank" href="/images/jared.jpg">
      <img src="/images/jared.jpg" alt="Jared" width="600" height="400">
    </a>
    <div class="desc"><a href="https://jbaza12.github.io/JaredsBlog//2024/03/19/heartdisease.html">Jared's Page</a></div>
  </div>
</div>


<div class="responsive">
  <div class="gallery">
    <a target="_blank" href="/images/nitin.jpeg">
      <img src="/images/nitin.jpeg" alt="Nitin" width="600" height="400">
    </a>
    <div class="desc"><a href="https://nitinsandiego.github.io/student//realEstate">Nitin's Page</a></div>
  </div>
</div>

<div class="responsive">
  <div class="gallery">
    <a target="_blank" href="/images/aidan.jpg">
      <img src="/images/aidan.jpg" alt="Northern Lights" width="600" height="400">
    </a>
    <div class="desc"><a href="https://aidanlau10.github.io/student2/employmentpredictor/">Aidan's Page</a></div>
  </div>
</div>



<div class="clearfix"></div>


</body>
</html>
