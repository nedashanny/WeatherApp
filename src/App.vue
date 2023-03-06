<script >

</script>

<template>
  <nav class="navbar bg-body-tertiary">
    <div class="container">
      <a class="navbar-brand">Accu</a>
      <form class="d-flex" role="search">
        <input class="form-control me-2" type="search" placeholder="Location" aria-label="Location">
        <button class="btn btn-outline-success" type="submit">Search</button>
      </form>
    </div>
  </nav>
  <div class="container">
    <p class="fs-3 text-capitalize fw-bold">current</p>
    <div class="card m-1" style="width: 10rem;">
      <img src="./assets/icons/1-s.png" class="card-img-top mt-1" alt="...">
      <div class="card-body">
        <h5 class="card-title">Cloudy</h5>
        <p class="fs-4">17 C</p>
        <p class="text-muted fs-6">01/11/12 - 20:20</p>
        <a href="#" class="btn btn-primary text-capitalize">view website</a>
      </div>
    </div>
    <p class="fs-3 text-capitalize fw-bold">hourly</p>
    <div class="w-100 overflow-scroll" style="white-space: nowrap;">
      <!-- 1 -->
      <div class="card m-1 d-lg-inline-block" style="width: 10rem;">
        <div class="card-header">3:00 AM</div>
        <img src="./assets/icons/1-s.png" class="card-img-top mt-1" alt="...">
        <div class="card-body">
          <h5 class="card-title">Cloudy</h5>
          <p class="fs-4">17 C</p>
        </div>
      </div>
    </div>
    <p class="fs-3 text-capitalize fw-bold mt-1">daily</p>
    <div class="w-100 overflow-scroll" style="white-space: nowrap;">
      <!-- 1 -->
      <div class="card m-1 d-lg-inline-block" style="width: 10rem;">
        <div class="card-header">Sunday</div>
        <img src="./assets/icons/1-s.png" class="card-img-top mt-1" alt="...">
        <div class="card-body">
          <h5 class="card-title">Cloudy</h5>
          <p class="fs-4">17 C</p>
        </div>
      </div>
    </div>
  </div>
  
</template>