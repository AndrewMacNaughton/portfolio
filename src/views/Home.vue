<template>
  <div class="about">
    <div>
      <h3><span>Andrew MacNaughton - Lead Front End Engineer</span></h3>
    </div>
    <div class="grid">
      <div>
        <p>
          Hi there! I’m Andy, a dedicated and passionate Lead Front-End Developer with a deep love for crafting seamless, intuitive web experiences using Vue.js. With 10+ years of
          experience in the industry, I’ve honed my skills in building dynamic and responsive interfaces that not only look great but also function flawlessly.
        </p>
        <p>
          Over the years, I've embraced Vue.js as my framework of choice due to its elegance, flexibility, and powerful features that streamline development and enhance
          performance. As a Vue.js advocate, I thrive on leveraging its capabilities to create innovative solutions and drive projects forward.
        </p>
        <p>
          In my role as a lead developer, I’m passionate about mentoring and guiding my team to excel and grow. I believe in fostering a collaborative environment where creativity
          and technical expertise are easily blended. From architecting scalable applications to optimizing performance and ensuring best practices, I’m committed to delivering
          high-quality results.
        </p>
        <p>
          I’m always eager to learn and adapt, driven by the ever-evolving landscape of front-end development. Let’s connect and explore how we can create amazing web experiences
          together!
          <a href="mailto:andy@andrewmacnaughton.xyz">Email me here!</a>
        </p>
        <p>This is a work in progess, but feel free to check out some the reusable components I have created on the compoenents tab</p>
      </div>
      <div class="image-wrapper">
        <div class="image">
          <img src="./../assets/me.jpg" />
        </div>
      </div>
    </div>
  </div>
</template>
<script setup></script>
<style lang="scss" scoped>
a {
  text-decoration: none;
  border-bottom: dotted 1px;
  color: inherit;
  &:hover {
    border-bottom: solid 1px;
  }
}
.about {
  padding: 32px;
}
h3 span {
  border-bottom: dotted 1px;
}
.image {
  display: flex;
  align-items: center;
  justify-content: center;
  img {
    width: 300px;
    border-radius: 16px;
  }
}
.grid {
  display: grid;
  grid-template-columns: 100%;
  @include breakpoint(medium) {
    grid-template-columns: 60% 40%;
  }
}
</style>
