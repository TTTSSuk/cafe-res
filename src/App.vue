<script setup>
import { ref } from 'vue';

const fontAwesomeLink = ref('https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.2.0/css/all.min.css');
const partyIconClass = ref('fas fa-glass-cheers');

const travellist = ref ([
  {name:'ท่าช้าง คาเฟ่', style:'ร้านอาหารกึ่งผับ กึ่งบาร์', service:'อาหาร เครื่องดื่ม ดนตรี',time:'17:00-23:59', img:"https://p-u.popcdn.net/event_details/posters/000/008/856/original/cfe1b94d03c5137faf1500f98117421e9f2ac9db.jpg?1590662796"},
  {name:'THE GOOD VIEW Bar & Restaurant', style:'ร้านอาหารกึ่งบาร์', service:'อาหาร เครื่องดื่ม ดนตรี คาราโอเกะ',time:'10:00-01:00', img:"https://admin.bangkokbanksme.com/uploads/topics/16744511306957.jpg"},
  {name:'The Riverside Bar & Restaurant' , style:'ร้านอาหาร กึ่งบาร์', service:'อาหาร เครื่องดื่ม ดนตรี',time:'10:00-01:00', img:"https://th.bing.com/th/id/R.ab9802205edc889cdc62f84af245316c?rik=EUFmNFFfO91eJA&riu=http%3a%2f%2fwww.dooasia.com%2fwp-content%2fuploads%2f2018%2f05%2fThe-RiverSide-%e0%b9%80%e0%b8%8a%e0%b8%b5%e0%b8%a2%e0%b8%87%e0%b9%83%e0%b8%ab%e0%b8%a1%e0%b9%88-7-696x464.jpg&ehk=IfTrEAF8BEuE9hJhtMKHQNYZmV%2boSJEesY%2fzQmFNYoo%3d&risl=&pid=ImgRaw&r=0"},
  {name:'WarmUp Cafe', style:'ร้านอาหารนั่งชิลล์', service:'อาหาร เครื่องดื่ม ดนตรี',time:'18:00-01:00', img:"https://d136usn7jnoe61.cloudfront.net/pictures/21526/s2n_0002_p1d06tc4oou8jlkvr0l1mag1grj6.jpg"},
  {name:'ตะวันแดง มหาชน', style:'ผับ', service:'อาหาร เครื่องดื่ม ดนตรี',time:'19:00-02:00', img:"https://th.bing.com/th/id/R.6fd3de7109e6a325c0d72efb53de2050?rik=Aa1zQxZXAWtxSg&riu=http%3a%2f%2ftawandang.com%2fwp-content%2fuploads%2f2017%2f08%2fTWDjang-Chaeng-Wattana.jpg&ehk=WHcn32JlAx8ZTvDdAHwMb81EWoU%2fiVLDtNNd6K%2bP6Ao%3d&risl=&pid=ImgRaw&r=0"},
  {name:'ฮอม บาร์', style:'อาหารกึ่งบาร์', service:'อาหาร เครื่องดื่ม ดนตรี ดีเจ',time:'17:00-24:00', img:"https://www.tripgether.com/wp-content/uploads/tripgetter/Hom_05.jpg"},
]);
const bookinglist = ref([]);
const tableQuantity = ref(Array(travellist.value.length).fill(0));
const bookingData = ref({
  name: '',
  phone: '',
  date: '',
  time: '',
});
const showBookingForm = ref(false);

function toggleBookingForm(index) {
  if (tableQuantity.value[index] > 0) {
    showBookingForm.value = !showBookingForm.value; 
    bookingData.value.index = index; 
  }
}

function submitBooking() {
  const index = bookingData.value.index;
  if (showBookingForm.value && typeof index !== 'undefined') {
    bookinglist.value.push({
      restaurant: travellist.value[index].name,
      tableQuantity: tableQuantity.value[index],
      bookingData: { ...bookingData.value },
    });
    tableQuantity.value[index] = 0;
    showBookingForm.value = false;
    delete bookingData.value.index; 
  }
}
function cancelBooking() {
  bookingData.value = {
    name: '',
    phone: '',
    date: '',
    time: '',
  };
  showBookingForm.value = false;
}
</script>

<template>
  <div>
    <link :href="fontAwesomeLink" rel="stylesheet">
    <h1><i :class="partyIconClass"></i> Please select your preferred restaurant!</h1>
<p class="note">-! กรุณาระบุจำนวนโต๊ะที่จอง แล้วกดปุ่มจองโต๊ะเพื่อกรอกข้อมูล !-</p>
</div>

<div class="container">
  <div class="row">
    <div class="col-md-8">
      <div class="row row-cols-1 row-cols-md-3 g-4">
        <div class="col" v-for="(restaurant, index) in travellist" :key="index">
          <div class="card-container">
            <div :class="['card', 'h-100', 'mx-auto', 'custom-card-bg']" style="width: 18rem;">
              <img v-if="restaurant.img" :src="restaurant.img" class="card-img-top" alt="Cafe" />
              <div :class="['card-body', 'd-flex', 'flex-column', 'align-items-center']">
                <h5 class="card-title"><b>{{ restaurant.name }}</b></h5>
                <p class="card-text">
                  <span style="text-align: left; display: inline-block;">
                    <b>Style:</b> {{ restaurant.style }} <br>
                    <b>Service:</b> {{ restaurant.service }} <br>
                    <b>Opening:</b> {{ restaurant.time }} <br>
                  </span>
                  <span class="mt-2">
                    <b>จำนวนโต๊ะที่จอง</b> <input type="number" v-model="tableQuantity[index]" />
                  </span>
                </p>
                <a class="btn btn-primary" @click="toggleBookingForm(index)">จองโต๊ะ</a>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  <div class="col-md-4">
    <div class="table-caption text-center">รายการจองโต๊ะ📃</div>
    <table class="table table-danger table-striped table-hover " v-if="bookinglist.length > 0">
  
      <!-- Booking table as before -->
      <thead>
    <tr>
      <th scope="col">ชื่อคนจอง</th>
      <th scope="col">ร้าน</th>
      <th scope="col">จำนวนโต๊ะที่จอง</th>
      <th scope="col">เบอร์โทร</th>
      <th scope="col">วันที่</th>
      <th scope="col">เวลา</th>
    </tr>
  </thead>
  <tbody class="table-group-divider ">
    <tr v-for="(booking, index) in bookinglist" :key="index">
      <td>{{ booking.bookingData.name }}</td>
      <td>{{ booking.restaurant }}</td>
      <td>{{ booking.tableQuantity }}</td>
      <td>{{ booking.bookingData.phone }}</td>
      <td>{{ booking.bookingData.date }}</td>
      <td>{{ booking.bookingData.time }}</td>
    </tr>
  </tbody> 
    </table> 
  </div>
  </div>
</div>

    <div class="modal-background" v-if="showBookingForm">
    <div class="floating-from">
      <h2>กรุณากรอกข้อมูลการจองโต๊ะ</h2>
      <form class="row g-3" @submit.prevent="submitBooking">
        <!-- Booking form inputs -->
        <div class="col-md-6">
          <label for="name" class="form-label ">ชื่อคนจอง</label>
          <input type="text" class="form-control border-danger" placeholder="ชื่อ-นามสกุล" id="name" v-model="bookingData.name" required />
        </div>
        <div class="col-md-6">
          <label for="phone" class="form-label">เบอร์โทร</label>
          <input type="tel" class="form-control border-danger" placeholder="0xxxxxxxxx" id="phone" v-model="bookingData.phone" required />
        </div>
        <div class="col-md-6">
          <label for="date" class="form-label">วันที่</label>
          <input type="date" class="form-control border-danger" id="date" v-model="bookingData.date" required />
        </div>
        <div class="col-md-6">
          <label for="time" class="form-label">เวลา</label>
          <input type="time" class="form-control border-danger" id="time" v-model="bookingData.time" required />
        </div>
        <div class="col-12">        
            <button type="submit" class="btn btn-primary">ยืนยันการจอง</button>
            <button type="button" class="btn btn-secondary" style="margin-left: 10px" @click="cancelBooking">ยกเลิก</button>
        </div>
      </form>
    </div>
  </div>
</template>