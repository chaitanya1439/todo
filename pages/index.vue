<template>
  <head>
    <link rel="stylesheet"
      href="https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined:opsz,wght,FILL,GRAD@24,400,0,0" />
  </head>

  <body>
    <div class="nav">
      <button>New List</button>
      <div class="tab">
        <button class="tablinks">List</button>
        <button class="tablinks">Scheduled</button>
        <button class="tablinks">Today</button>
      </div>
      <div class="iconic">
        <button><span class="material-symbols-outlined">
            done
          </span></button>
        <button><span class="material-symbols-outlined">
            search
          </span></button>
      </div>
    </div>
    <div class="new"><a href="/personal">
      <img src="/newpage.png" alt="newpage" style="margin-left: 50px;margin-top: 100px;"></a>
      <p>
    <h4 style="margin-left: 120px;">Personal</h4>
      <h5 style="margin-left: 110px;">On This Page</h5>
      </p>
    </div>
  </body>
</template>
<style>
.nav {
  overflow: hidden;
  display: flex;
}

.tab {
  position: relative;
  overflow: hidden;
  border: 1px solid #ddd;
  background-color: #f1f1f1;
  margin-left: 800px;
  width: 200px;
  display: flex;
}

.nav button {
  background-color: inherit;
  float: left;
  border: none;
  outline: none;
  cursor: pointer;
  padding: 14px 15px 16px;
  transition: 0.3s;
}

.nav button:hover {
  background-color: #ddd;
}

.nav button.active {
  background-color: #ccc;
}

.iconic {
  margin-left: 600px;
}</style>
