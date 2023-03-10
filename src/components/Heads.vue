<template>
  <section class="top">
    <a href="/" title="На главную страницу" class="logo">
      <span class="logo-text-container">
			<span class="logo-text">Битрикс</span>
        <span class="logo-color">24</span>
      </span>
    </a>
  </section>
</template>

<script>
export default {
  name: 'heads',
  data () {
    return {
      vociceOpen:true
    }
  }
}
</script>

<style scoped>
.logo {
  display: -ms-inline-flexbox;
  display: -webkit-inline-flex;
  display: inline-flex;
  align-items: center;
  font-size: 24px;
  font-family: var(--ui-font-family-secondary,var(--ui-font-family-open-sans));
  font-weight: var(--ui-font-weight-semi-bold,600);
  padding-right: 10px;
  min-width: 185px;
  vertical-align: middle;
  white-space: nowrap;
  transition: opacity .2s;
  text-decoration: none;
}
.logo-color {
  flex-grow: 1;
  flex-shrink: 1;
  flex-basis: 0;
  color: #2fc6f7;
}
.logo-text-container {
  flex-wrap: nowrap;
  align-items: center;
  display: flex;
}
.logo-text {
  color: #fff;
  display: block;
  margin-right: 7px;
  max-width: 240px;
  overflow: hidden;
  position: relative;
  flex-grow: 1;
  flex-shrink: 1;
}
  .top {
    height: 50px;
    line-height: 50px;
    position: absolute;
    top: 0px;
    left: 0px;
    right: 0px;
    padding: 5px 10px;
  }

  .top .head {
    width: 50px;
    height: 50px;
    float: left;
    border-radius: 100%;
  }

  .top .name {
    font-size: 18px;
    color: #444;
    margin-left: 10px;
  }

  .top .voice {
    width: 30px;
    height: 50px;
    float: right;
    text-align: center;
  }
</style>
