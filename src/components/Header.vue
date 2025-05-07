<script setup></script>

<template>
  <header>
    <h1>Svenssons Hantverk AB</h1>
    <div>
      <img src="./../../public/pfp.png" alt="profile picture" />
      <div>
        <p>Namn Namnsson</p>
        <p>Admin</p>
      </div>
    </div>
  </header>
</template>

<style scoped>
header {
  background-color: #219df5;
  width: 100%;
  height: 80px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 6rem 0 5rem;

  h1 {
    color: white;
    letter-spacing: 2px;
  }

  div {
    align-items: center;
    font-size: 14px;
    color: white;
    display: flex;
    gap: 14px;

    img {
      height: 48px;
    }

    div {
      flex-direction: column;
      align-items: flex-start;
      display: flex;
      margin-top: 4px;
      gap: 2px;

      p:nth-child(1) {
        font-weight: bolder;
      }
    }
  }
}
</style>
