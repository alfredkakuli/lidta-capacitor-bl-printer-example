
<script setup>
import { onMounted, ref } from 'vue';
import html2canvas from 'html2canvas';
import { LidtaCapacitorBlPrinter } from 'lidta-capacitor-bl-printer';
const devices = ref([]);
const connectedPrinter = ref(null);
const content = ref(null);

const listDevices = async () => {
  await LidtaCapacitorBlPrinter.getPairedDevices().then((result) => {
      devices.value = result.devices || [];
  }).catch((error) => {
    alert(error);
  });
}       


const printReceipt = async (device) => {
  console.log(device);
 await  LidtaCapacitorBlPrinter.connect({ address: device.address }).then(async () => {
   connectedPrinter.value = device.name;
   await  html2canvas(
      content.value,
    ).then(async (canvas) => {
    const base64Image =  canvas.toDataURL("image/png");
    LidtaCapacitorBlPrinter.printBase64({
      align: 1,
      msg: base64Image,
    }).then(() => {
     console.log('Printed')
    LidtaCapacitorBlPrinter.disconnect();
   }).catch((error) =>
       alert(error)).finally(async () => {

       });
  }).catch((error) => alert(error));
  }).catch((error) => alert(error))
};
</script>

<template>
<div class=" mt-8">


  <div  class="text-center" >
   
    <VBtn color="warning" class="text-center" variant="tonal"  @click="listDevices">
      <VIcon icon="mdi-printer" size="24" color="info" ></VIcon>
      List Connected Devices</VBtn>
  </div>


 <div class="text-center">
   <VBtn variant="tonal" class="mt-1" v-for="device in devices" :key="device" @click="printReceipt(device)" color="indigo">
    
  <VIcon icon="mdi-printer" ></VIcon>  Print on {{ device.name }} 
  
  </VBtn>

 </div>

  <div class="text-center">
    connected {{ connectedPrinter?.name }}
  </div>


  <div  ref="content" id="content">
 <div style="font-family: Arial, sans-serif; margin: 0; padding: 0;">
  <div style="padding: 10px; width: 100%;">
    <!-- Header with logo and company details -->
    <div style="text-align: center; margin-bottom: 10px;">
      <img  height="80" width="80" src="@/assets/logo.png"  />
      <div style="font-size: 14px; text-align: center; margin-top: 5px;">
        <h2>Lidta Tech</h2>
        <h2>72  Kiserian Kajiado North., Kenya</h2>
        <h2>Email: info@lidta.com.com</h2>
      </div>
    </div>


    <hr >

    <!-- Invoice info -->
    <div style="margin-top: 15px; font-size: 12px;">
      <div style="margin-bottom: 5px;">
        <h2><strong>Invoice:</strong> INV-123456<strong>فاتورة</strong></h2>
        <h2><strong>Date:</strong> 01/02/2025</h2>
      </div>
      <div style="display: flex; justify-content: space-between; margin-bottom: 5px;">
        <h2><strong>Bill To: John Doe</strong></h2>
      </div>
    
      <div style="display: flex; justify-content: space-between; margin-bottom: 5px;">
        <h2>456 Customer Ave., City</h2>
      </div>
      <div style="display: flex; justify-content: space-between; margin-bottom: 5px;">
        <h2>Email: john.doe@example.com</h2>
      </div>
    </div>
    <hr>
    <table style="width: 100%; margin-top: 10px; font-size: 18px;" >
      <thead>
        <tr>
          <th style="padding: 5px 0; border-bottom: 1px solid #ddd; text-align: left;">Description</th>
          <th style="padding: 5px 0; border-bottom: 1px solid #ddd; text-align: left;">Price</th>
          <th style="padding: 5px 0; border-bottom: 1px solid #ddd; text-align: left;">Qty</th>
          <th style="padding: 5px 0; border-bottom: 1px solid #ddd; text-align: right;">Total</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td style="padding: 5px 0; border-bottom: 1px solid #ddd; text-align: left;">Product 1</td>
          <td style="padding: 5px 0; border-bottom: 1px solid #ddd; text-align: left;">$50.00</td>
          <td style="padding: 5px 0; border-bottom: 1px solid #ddd; text-align: left;">2</td>
          <td style="padding: 5px 0; border-bottom: 1px solid #ddd; text-align: right;">$100.00</td>
        </tr>
        <tr>
          <td style="padding: 5px 0; border-bottom: 1px solid #ddd; text-align: left;">Product 2</td>
          <td style="padding: 5px 0; border-bottom: 1px solid #ddd; text-align: left;">$30.00</td>
          <td style="padding: 5px 0; border-bottom: 1px solid #ddd; text-align: left;">1</td>
          <td style="padding: 5px 0; border-bottom: 1px solid #ddd; text-align: right;">$30.00</td>
        </tr>

         <tr>
          <td style="padding: 5px 0; border-bottom: 1px solid #ddd; text-align: left;">Product 3</td>
          <td style="padding: 5px 0; border-bottom: 1px solid #ddd; text-align: left;">$30.00</td>
          <td style="padding: 5px 0; border-bottom: 1px solid #ddd; text-align: left;">1</td>
          <td style="padding: 5px 0; border-bottom: 1px solid #ddd; text-align: right;">$30.00</td>
        </tr>

         <tr>
          <td style="padding: 5px 0; border-bottom: 1px solid #ddd; text-align: left;">Product 4</td>
          <td style="padding: 5px 0; border-bottom: 1px solid #ddd; text-align: left;">$30.00</td>
          <td style="padding: 5px 0; border-bottom: 1px solid #ddd; text-align: left;">1</td>
          <td style="padding: 5px 0; border-bottom: 1px solid #ddd; text-align: right;">$30.00</td>
        </tr>

         <tr>
          <td style="padding: 5px 0; border-bottom: 1px solid #ddd; text-align: left;">Product 5</td>
          <td style="padding: 5px 0; border-bottom: 1px solid #ddd; text-align: left;">$30.00</td>
          <td style="padding: 5px 0; border-bottom: 1px solid #ddd; text-align: left;">1</td>
          <td style="padding: 5px 0; border-bottom: 1px solid #ddd; text-align: right;">$30.00</td>
        </tr>

         <tr>
          <td style="padding: 5px 0; border-bottom: 1px solid #ddd; text-align: left;">Product 6</td>
          <td style="padding: 5px 0; border-bottom: 1px solid #ddd; text-align: left;">$30.00</td>
          <td style="padding: 5px 0; border-bottom: 1px solid #ddd; text-align: left;">1</td>
          <td style="padding: 5px 0; border-bottom: 1px solid #ddd; text-align: right;">$30.00</td>
        </tr>

         <tr>
          <td style="padding: 5px 0; border-bottom: 1px solid #ddd; text-align: left;">Product 7</td>
          <td style="padding: 5px 0; border-bottom: 1px solid #ddd; text-align: left;">$30.00</td>
          <td style="padding: 5px 0; border-bottom: 1px solid #ddd; text-align: left;">1</td>
          <td style="padding: 5px 0; border-bottom: 1px solid #ddd; text-align: right;">$30.00</td>
        </tr>
      </tbody>
    </table>

    <!-- Total -->
    <div style="text-align: right; font-size: 18px; margin-top: 10px;">
      <strong>Total: $130.00</strong>
    </div>
<hr>
    <!-- Footer -->
    <div style="text-align: center; margin-bottom: 70px;">
      <h2><strong>Thank you for your business!</strong></h2>
      <h2><strong> <a href="https://www.app.lidta.com" target="_blank">www.app.lidta.com</a> </strong></h2>
    </div>
  </div>

</div>
</div>
  
</div>
</template>


