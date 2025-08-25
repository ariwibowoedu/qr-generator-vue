<template>
  <div class="max-w-xl mx-auto text-center my-8 sh px-4 py-24">
    <h1 class="text-2xl font-semibold mb-4">Generate QR Code</h1>
    <p class="text-md text-gray-600">
      Make your link more easy to access by only scane the barcode.
    </p>
    <div class="flex items-center gap-2 my-4">
      <input
        v-model="qrValue"
        type="text"
        placeholder="Enter text or URL"
        class="my-4 py-2 px-4 border rounded w-full focus:outline-none focus:ring-indigo-500 text-sm"
      />
      <button
        @click="resetQrCode"
        class="px-4 py-2 rounded bg-gray-400 hover:opacity-50 transition-colors duration-200"
      >
        Reset
      </button>
    </div>

    <div class="flex flex-col md:flex-row justify-around items-center">
      <button
        :disabled="!qrValue"
        @click="generateQRCode"
        class="px-4 py-2 w-full tracking-wide md:w-auto mb-2 text-white rounded hover:bg-opacity-90"
        :class="
          qrValue
            ? 'bg-gray-400 hover:bg-opacity-90'
            : 'bg-gray-300 cursor-not-allowed'
        "
      >
        Generate
      </button>
      <button
        :disabled="!qrGenerate"
        @click="downloaQRCode"
        class="px-4 py-2 w-full tracking-wide md:w-auto mb-2 bg-blueberry text-white rounded hover:bg-opacity-90"
        :class="
          qrGenerate
            ? 'bg-blue-500 hover:bg-opacity-90'
            : 'bg-blue-700 cursor-not-allowed'
        "
      >
        Download
      </button>
    </div>

    <div class="mt-6 flex justify-center">
      <canvas ref="qrCanvas" class="mx-auto"></canvas>
    </div>
  </div>
  <hr />
  <footer class="max-w-xl mx-auto text-center my-8">
    <p class="text-md text-gray-600">
      &copy;
      <a href="https://ariwibowoedu.github.io/" target="_blank">
        ariwibowoedu.github.io
      </a>
    </p>
  </footer>
</template>

<script setup>
import { nextTick, ref } from "vue";
import QRCode from "qrcode";

const qrCanvas = ref(null);
const qrValue = ref("");
const qrGenerate = ref(false);

const generateQRCode = async () => {
  try {
    await nextTick();
    await QRCode.toCanvas(qrCanvas.value, qrValue.value, {
      color: {
        dark: "#000000",
        light: "#ffffff",
      },
      width: 300,
      errorCorrectionLevel: "H",
    });
    qrValue.value = "";
    qrGenerate.value = true;
  } catch (error) {
    console.error("Error render barcode:", error);
    qrGenerate.value = false;
  }
};

const resetQrCode = () => {
  qrValue.value = "";
  qrGenerate.value = false;
};

const downloaQRCode = () => {
  if (qrCanvas.value) {
    const link = document.createElement("a");
    link.href = qrCanvas.value.toDataURL("image/png");
    link.download = "qr-code.png";
    link.click();
  }
};
</script>

<style>
</style>