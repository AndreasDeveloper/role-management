<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@200;300;400;500;600&display=swap');

#app {
  font-family: 'Montserrat', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

body {
  margin: 0;
  padding: 0;
}

// Set Default
button, input, select, textarea {
  border: none;
  outline: none;
}

button {
  cursor: pointer;
  &:disabled {
    opacity: 0.5;
  }
}


input, select, textarea {
  border-bottom: 2px solid #3498db;
  padding-bottom: 5px;
  width: 20%;
  &::placeholder {
    font-size: 15px;
  }
  @media only screen and (max-width: 800px) {
    width: 90%;
  }
}
select {
  font-size: 15px;
}

h1, h2, h3, h4 {
  margin: 0;
  padding: 0;
}

.create-cta {
  background: #3498db !important;
  border-radius: 4px;
  color: #fff;
  text-transform: uppercase;
  padding: 10px 15px;
}
</style>
