<template>
  <section class="location">
    <p class="label">Wedding Location</p>

    <h3 class="place">Multnomah County Courthouse</h3>
    <p class="address">
      1200 SW 1st Ave<br />
      Portland, OR 97204
    </p>

    <!-- 지도 -->
    <div class="map-wrap">
      <iframe
        title="Wedding Location Map"
        loading="lazy"
        referrerpolicy="no-referrer-when-downgrade"
        src="https://www.google.com/maps?q=Multnomah+County+Courthouse,+Portland+OR&output=embed"
      />
    </div>

    <!-- 구글 지도 열기 -->
    <a
      class="map-link"
      href="https://maps.app.goo.gl/zpkWKxTZNU6amQ4i9"
      target="_blank"
      rel="noopener"
    >
      📍 Open in Google Maps
    </a>
  </section>
</template>

<style scoped>
.location {
  text-align: center;
  padding: 60px 16px;
  /* background: #f2f2f2; */
  background: #f9f9f9;
}

.label {
  font-size: 13px;
  letter-spacing: 0.12em;
  color: #777;
  margin-bottom: 8px;
}

.place {
  font-size: 20px;
  font-weight: 500;
  margin-bottom: 6px;
}

.address {
  font-size: 14px;
  color: #555;
  line-height: 1.4;
  margin-bottom: 20px;
}

.map-wrap {
  width: 100%;
  max-width: 420px;
  height: 240px;
  margin: 0 auto 16px;
  border-radius: 16px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
  overflow: hidden;
}

iframe {
  width: 100%;
  height: 100%;
  border: 0;
}

.map-link {
  display: inline-block;
  font-size: 14px;
  color: #222;
  text-decoration: underline;
}
</style>
