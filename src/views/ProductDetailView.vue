<template>
  <Toast />
  <body>
    <h1>Hello</h1>
    <div class="flex items-start space-x-4">
      <!-- Left Section: Thumbnails with Navigation -->
      <div class="relative flex flex-col items-center w-1/6">
        <!-- Previous Button -->
        <!-- <button class="absolute top-0 left-0 z-10 bg-gray-300 p-1 rounded-full shadow-md">
      <i class="fas fa-chevron-up"></i>
    </button> -->

        <!-- Thumbnails -->
        <div class="flex flex-col items-center space-y-2 overflow-hidden h-80">
          <div class="w-24 h-24 bg-gray-200 flex items-center justify-center">
            <img
              alt="Thumbnail image 1"
              class="w-20 h-20 object-contain"
              src=""
            />
          </div>
          <div class="w-24 h-24 bg-gray-200 flex items-center justify-center">
            <img
              alt="Thumbnail image 2"
              class="w-20 h-20 object-contain"
              src=""
            />
          </div>
          <div class="w-24 h-24 bg-gray-200 flex items-center justify-center">
            <img
              alt="Thumbnail image 3"
              class="w-20 h-20 object-contain"
              src=""
            />
          </div>
        </div>

        <!-- Next Button -->
        <!-- <button class="absolute bottom-0 left-0 z-10 bg-gray-300 p-1 rounded-full shadow-md">
      <i class="fas fa-chevron-down"></i>
    </button> -->
      </div>

      <!-- Main Image -->
      <div
        class="w-2/4 flex items-center justify-center relative"
        style="margin-left: -20px"
      >
        <div class="w-full h-full bg-gray-200 flex items-center justify-center">
          <img
            v-if="detailProduct"
            class="img-responsive rounded-tl-[15px] rounded-tr-[15px]"
            :src="getImageUrl(detailProduct.thumbnail)"
            alt="product-1"
          />
        </div>
        <div
          class="absolute top-2 left-2 bg-black bg-opacity-50 rounded-full p-2"
        >
          <i class="fas fa-heart text-white"></i>
        </div>
      </div>

      <!-- Right Section: Product Details -->
      <div class="w-1/3 p-4">
        <h1 class="text-xl font-semibold">{{ detailProduct?.name }}</h1>
        <p class="text-gray-500">SKU: {{ detailProduct?.serial }}</p>

        <!-- Price -->
        <div class="mt-4">
          <span
            class="bg-red-500 text-white text-xs font-semibold px-2 py-1 rounded"
            >-20%</span
          >
          <span class="text-2xl font-bold text-orange-500 ml-2">{{
            detailProduct?.sellPrice
          }}</span>
          <span class="text-gray-500 line-through ml-2">250.000đ</span>
        </div>
        <div class="mt-2 text-sm text-gray-500">
          <span class="text-green-500">Tình trạng: Còn hàng</span>
        </div>

        <!-- Options -->
        <div class="mt-4">
          <label class="block text-sm font-medium text-gray-700">Màu sắc</label>
          <div class="bb-pro-variation-contant">
            <ul
              v-if="
                detailProduct &&
                detailProduct.colors &&
                detailProduct.colors.length > 0
              "
              class="flex flex-wrap m-[-2px]"
            >
              <li
                v-for="(color, index) in detailProduct.colors"
                :key="index"
                class="my-[10px] mx-[2px] py-[2px] px-[15px] border-[1px] border-solid border-[#eee] rounded-[10px] cursor-pointer"
                :class="{ 'active-variation': selectedColor === color }"
                @click="selectedColor = color"
              >
                <span
                  class="font-Poppins text-[#686e7d] font-light text-[14px] leading-[28px] tracking-[0.03rem]"
                >
                  {{ color }}
                </span>
              </li>
            </ul>
            <p v-else>Không có màu sắc cho sản phẩm này.</p>
          </div>
        </div>
        <div class="mt-4">
          <label class="block text-sm font-medium text-gray-700"
            >Kích thước</label
          >
          <select
            class="mt-1 block w-full py-2 px-3 border border-gray-300 bg-white rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"
          >
            <option>92 × 99 × 165 mm</option>
            <option>100 x 100 x 200 mm</option>
          </select>
        </div>

        <!-- Quantity Selector -->
        <div class="mt-4 flex items-center">
          <label class="block text-sm font-medium text-gray-700 mr-4"
            >Số lượng</label
          >
          <div class="bb-single-qty flex flex-wrap m-[-2px]">
            <div
              class="qty-plus-minus m-[2px] w-[85px] h-[40px] py-[7px] border-[1px] border-solid border-[#eee] overflow-hidden relative flex items-center justify-between bg-[#fff] rounded-[10px]"
            >
              <button
                class="qty-minus w-[25px] h-[25px] flex items-center justify-center text-[#777] hover:text-[#6c7fd8]"
                @click="quantity > 1 ? quantity-- : null"
              >
                <i class="ri-subtract-line"></i>
              </button>
              <input
                type="text"
                class="qty-input w-[35px] h-full border-none text-center font-Poppins text-[14px] text-[#777]"
                v-model="quantity"
                readonly
              />
              <button
                class="qty-plus w-[25px] h-[25px] flex items-center justify-center text-[#777] hover:text-[#6c7fd8]"
                @click="quantity++"
              >
                <i class="ri-add-line"></i>
              </button>
            </div>
          </div>
        </div>

        <!-- Actions -->
        <div class="mt-4 flex space-x-2">
          <button class="bg-orange-500 text-white py-2 px-4 rounded">
            Mua hàng
          </button>
          <a
            href="javascript:void(0)"
            class="bb-btn-2 transition-all duration-[0.3s] ease-in-out h-[40px] flex font-Poppins leading-[28px] tracking-[0.03rem] py-[6px] px-[25px] text-[14px] font-normal text-[#fff] bg-[#6c7fd8] rounded-[10px] border-[1px] border-solid border-[#6c7fd8] hover:bg-transparent hover:border-[#3d4750] hover:text-[#3d4750]"
            @click="handleAddToCart"
          >
            Thêm vào giỏ hàng
          </a>
        </div>

        <!-- Features List -->
        <ul class="mt-4 text-sm text-gray-700 list-disc list-inside">
          <p class="text-gray-500 mb-4">
            Mô tả: {{ detailProduct?.description }}
          </p>
        </ul>
      </div>
    </div>

    <div id="app" class="bg-gray-100">
      <main class="container mx-auto px-4 py-6">
        <div class="flex flex-col lg:flex-row">
          <div class="lg:w-1/2"></div>
        </div>

        <!-- Product Tabs -->
        <div class="mx-auto mt-4" id="app">
          <div class="bg-white rounded-lg shadow-md p-4">
            <div class="border-b border-gray-300 mb-4">
              <ul class="flex space-x-4 text-gray-600">
                <li
                  class="py-2 px-4 border-b-2 border-transparent hover:border-black cursor-pointer"
                >
                  Giới thiệu
                </li>
                <li class="py-2 px-4 border-b-2 border-black cursor-pointer">
                  Thông số kỹ thuật
                </li>
                <li
                  class="py-2 px-4 border-b-2 border-transparent hover:border-black cursor-pointer"
                >
                  Đánh giá
                </li>
                <li
                  class="py-2 px-4 border-b-2 border-transparent hover:border-black cursor-pointer"
                >
                  Sản phẩm liên quan
                </li>
              </ul>
            </div>
            <div class="flex flex-col lg:flex-row">
              <div class="flex-1">
                <h1 class="text-2xl font-bold mb-4">Thiết kế đầy cá tính</h1>
                <p class="mb-4">
                  Hệ thống loa Logitech Z906 bao gồm 1 loa bass công suất 165W,
                  5 loa vệ tinh mỗi loa công suất 67W, 1 bộ khuyếch đại âm
                  thanh, điều khiển từ xa. Loa có thiết kế phong cách hiện đại,
                  đầy cá tính, đáp ứng mọi nhu cầu âm thanh của bạn.
                </p>
                <div
                  class="bg-gray-200 h-64 flex items-center justify-center mb-4"
                >
                  <img
                    alt="Placeholder image for product"
                    class="h-full w-full object-cover"
                    height="400"
                    src="https://placehold.co/600x400"
                    width="600"
                  />
                </div>
                <h2 class="text-xl font-bold mb-2">
                  Loa bass âm thanh siêu trầm
                </h2>
                <p class="mb-4">
                  Logitech Z906 có loa bass kích thước màng loa 8 inchs, công
                  suất lên tới 165W, cho âm thanh siêu trầm. Z906 có thể đáp ứng
                  được tất cả các bản nhạc có âm thanh cực nhỏ, nhỏ, lớn hay cực
                  lớn.
                </p>
                <h2 class="text-xl font-bold mb-2">Điều khiển từ xa</h2>
                <p>
                  Logitech Z906 trang bị điều khiển từ xa qua hồng ngoại, thiết
                  nhỏ gọn, sử dụng 3 pin AAA, có thể kết hợp với các thiết bị
                  phát âm thanh như: TV, DVD, DVR, Blu-ray™, Xbox 360,
                  PLAYSTATION 3, iPod...Điều khiển này có chức năng như: tăng
                  giảm âm thanh, tùy chọn các kênh, lựa chọn kiểu âm thanh (2.1
                  – 4.1 và 3D).
                </p>
              </div>
              <div class="w-full lg:w-1/3 lg:ml-4 mt-4 lg:mt-0">
                <div class="bg-white border border-gray-300 rounded-lg p-4">
                  <h2 class="text-xl font-bold mb-4">Thông số kỹ thuật</h2>
                  <div class="space-y-2">
                    <div class="flex justify-between bg-gray-100 p-2 rounded">
                      <span class="font-semibold"> Cổng kết nối </span>
                      <span> 2 cổng Optical, Coaxial, RCA, 3.5mm </span>
                    </div>
                    <div class="flex justify-between p-2 rounded">
                      <span class="font-semibold"> Thương hiệu </span>
                      <span> Logitech </span>
                    </div>
                    <div class="flex justify-between bg-gray-100 p-2 rounded">
                      <span class="font-semibold"> Sản xuất tại </span>
                      <span> Trung Quốc </span>
                    </div>
                    <div class="flex justify-between p-2 rounded">
                      <span class="font-semibold"> Kích thước </span>
                      <span>
                        92 x 99 x 165 mm | 92 x 165 x 99 mm | 318 x 280 x 292 mm
                        | 179 x 42 x 110 mm | 18 x 42 x 110 mm
                      </span>
                    </div>
                    <div class="flex justify-between bg-gray-100 p-2 rounded">
                      <span class="font-semibold"> Sản xuất tại </span>
                      <span> Trung Quốc </span>
                    </div>
                    <div class="flex justify-between p-2 rounded">
                      <span class="font-semibold"> Màu </span>
                      <span> Đen, Trắng, Đỏ, Bạc, Vàng </span>
                    </div>
                    <div class="flex justify-between bg-gray-100 p-2 rounded">
                      <span class="font-semibold"> Kích thước </span>
                      <span> 27.6 x 32.7 x 29.3 cm </span>
                    </div>
                    <div class="flex justify-between p-2 rounded">
                      <span class="font-semibold"> Trọng lượng </span>
                      <span> 5.9 kg </span>
                    </div>
                    <div class="flex justify-between bg-gray-100 p-2 rounded">
                      <span class="font-semibold"> Điều khiển </span>
                      <span> Nút bấm và công tắc vật lý </span>
                    </div>

                    <div class="flex justify-between bg-gray-100 p-2 rounded">
                      <span class="font-semibold"> Công suất </span>
                      <span> 100 W </span>
                    </div>
                  </div>
                  <button
                    class="mt-4 w-full bg-gray-200 text-gray-700 py-2 rounded-lg"
                  >
                    Xem thông số chi tiết
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- Existing Reviews -->
        <div class="w-full p-4" id="app">
          <div
            class="w-full bg-white p-6 rounded-lg shadow-md border border-gray-200"
          >
            <h2 class="text-xl font-semibold mb-4">
              Hãy đánh giá cho
              <span class="text-orange-600">
                Loa vi tính Logitech Z906 5.1
              </span>
            </h2>
            <div class="mb-4">
              <label class="block text-gray-700 mb-2">
                Đánh giá của bạn cho sản phẩm này
              </label>
              <div class="flex items-center mb-2">
                <i
                  class="far fa-star text-2xl text-gray-400 cursor-pointer"
                ></i>
                <i
                  class="far fa-star text-2xl text-gray-400 cursor-pointer"
                ></i>
                <i
                  class="far fa-star text-2xl text-gray-400 cursor-pointer"
                ></i>
                <i
                  class="far fa-star text-2xl text-gray-400 cursor-pointer"
                ></i>
                <i
                  class="far fa-star text-2xl text-gray-400 cursor-pointer"
                ></i>
              </div>
              <textarea
                class="w-full p-2 border border-gray-300 rounded-md"
                placeholder="Xin mời chia sẻ một số cảm nhận (tối thiểu 10 ký tự)"
                rows="4"
              ></textarea>
            </div>
            <button class="bg-orange-600 text-white px-4 py-2 rounded-md">
              Gửi đánh giá
            </button>
            <div class="mt-6">
              <h3 class="text-lg font-semibold mb-2">
                Đánh giá
                <span class="text-orange-600"> (2) </span>
              </h3>
              <div class="flex items-center justify-between mb-4">
                <button class="flex items-center text-gray-600">
                  <i class="fas fa-filter mr-2"></i>
                  Bộ lọc
                </button>
                <div class="flex items-center">
                  <span class="text-gray-600 mr-2"> Sắp xếp theo </span>
                  <select class="border border-gray-300 rounded-md p-1">
                    <option>Mới</option>
                  </select>
                </div>
              </div>
              <div class="space-y-4">
                <div
                  class="flex items-start space-x-4 p-4 bg-gray-100 rounded-md"
                >
                  <img
                    alt="User avatar"
                    class="w-12 h-12 rounded-full"
                    height="50"
                    src=""
                    width="50"
                  />
                  <div class="flex-1">
                    <div class="flex items-center mb-1">
                      <i class="fas fa-star text-yellow-500"></i>
                      <i class="fas fa-star text-yellow-500"></i>
                      <i class="fas fa-star text-yellow-500"></i>
                      <i class="fas fa-star text-yellow-500"></i>
                      <i class="fas fa-star text-yellow-500"></i>
                    </div>
                    <p class="text-gray-700 font-semibold">Đánh giá của bạn</p>
                    <p class="text-gray-500 text-sm">dd/MM/yyyy</p>
                    <a class="text-blue-500 text-sm" href="#"> Chỉnh sửa </a>
                  </div>
                </div>
                <div
                  class="flex items-start space-x-4 p-4 bg-gray-100 rounded-md"
                >
                  <img
                    alt="User avatar"
                    class="w-12 h-12 rounded-full"
                    height="50"
                    src=""
                    width="50"
                  />
                  <div class="flex-1">
                    <div class="flex items-center mb-1">
                      <i class="fas fa-star text-yellow-500"></i>
                      <i class="fas fa-star text-yellow-500"></i>
                      <i class="fas fa-star text-yellow-500"></i>
                      <i class="fas fa-star text-yellow-500"></i>
                      <i class="fas fa-star text-yellow-500"></i>
                    </div>
                    <p class="text-gray-700 font-semibold">
                      Đánh giá của khách hàng
                    </p>
                    <p class="text-gray-500 text-sm">dd/MM/yyyy</p>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- Related Products Section -->
        <div class="p-4" id="app">
          <div class="w-full">
            <h2 class="text-xl font-bold mb-4">Sản phẩm liên quan</h2>
            <div class="relative">
              <div class="flex items-center">
                <button
                  class="absolute left-0 bg-teal-600 text-white rounded-full p-2"
                >
                  <i class="fas fa-chevron-left"> </i>
                </button>
                <div class="flex overflow-x-auto space-x-4 px-8 w-full">
                  <div
                    class="relative bg-white border rounded-lg shadow-md p-4 w-64 flex-shrink-0"
                  >
                    <div
                      class="absolute top-0 right-0 bg-red-500 text-white text-xs font-bold px-2 py-1"
                    >
                      -20%
                    </div>
                    <img
                      alt="Placeholder image"
                      class="w-full h-40 object-cover mb-4"
                      height="150"
                      src="https://placehold.co/150x150"
                      width="150"
                    />
                    <h3 class="text-sm font-semibold">
                      Pin sạc dự phòng Energizer
                    </h3>
                    <div class="text-lg font-bold text-red-500">199.999đ</div>
                    <div class="text-sm text-gray-500 line-through">
                      250.000đ
                    </div>
                    <button
                      class="mt-2 bg-orange-500 text-white text-sm font-bold py-2 px-4 rounded"
                    >
                      Mua ngay
                    </button>
                    <button class="absolute bottom-4 right-4 text-gray-500">
                      <i class="far fa-heart"> </i>
                    </button>
                  </div>
                  <div
                    class="relative bg-white border rounded-lg shadow-md p-4 w-64 flex-shrink-0"
                  >
                    <div
                      class="absolute top-0 right-0 bg-red-500 text-white text-xs font-bold px-2 py-1"
                    >
                      -20%
                    </div>
                    <img
                      alt="Placeholder image"
                      class="w-full h-40 object-cover mb-4"
                      height="150"
                      src="https://placehold.co/150x150"
                      width="150"
                    />
                    <h3 class="text-sm font-semibold">
                      Pin sạc dự phòng Energizer
                    </h3>
                    <div class="text-lg font-bold text-red-500">199.999đ</div>
                    <div class="text-sm text-gray-500 line-through">
                      250.000đ
                    </div>
                    <button
                      class="mt-2 bg-orange-500 text-white text-sm font-bold py-2 px-4 rounded"
                    >
                      Mua ngay
                    </button>
                    <button class="absolute bottom-4 right-4 text-gray-500">
                      <i class="far fa-heart"> </i>
                    </button>
                  </div>
                  <div
                    class="relative bg-white border rounded-lg shadow-md p-4 w-64 flex-shrink-0"
                  >
                    <div
                      class="absolute top-0 right-0 bg-red-500 text-white text-xs font-bold px-2 py-1"
                    >
                      -20%
                    </div>
                    <img
                      alt="Placeholder image"
                      class="w-full h-40 object-cover mb-4"
                      height="150"
                      src="https://placehold.co/150x150"
                      width="150"
                    />
                    <h3 class="text-sm font-semibold">
                      Pin sạc dự phòng Energizer
                    </h3>
                    <div class="text-lg font-bold text-red-500">199.999đ</div>
                    <div class="text-sm text-gray-500 line-through">
                      250.000đ
                    </div>
                    <button
                      class="mt-2 bg-orange-500 text-white text-sm font-bold py-2 px-4 rounded"
                    >
                      Mua ngay
                    </button>
                    <button class="absolute bottom-4 right-4 text-gray-500">
                      <i class="far fa-heart"> </i>
                    </button>
                  </div>
                  <div
                    class="relative bg-white border rounded-lg shadow-md p-4 w-64 flex-shrink-0"
                  >
                    <div
                      class="absolute top-0 right-0 bg-red-500 text-white text-xs font-bold px-2 py-1"
                    >
                      -20%
                    </div>
                    <img
                      alt="Placeholder image"
                      class="w-full h-40 object-cover mb-4"
                      height="150"
                      src="https://placehold.co/150x150"
                      width="150"
                    />
                    <h3 class="text-sm font-semibold">
                      Pin sạc dự phòng Energizer
                    </h3>
                    <div class="text-lg font-bold text-red-500">199.999đ</div>
                    <div class="text-sm text-gray-500 line-through">
                      250.000đ
                    </div>
                    <button
                      class="mt-2 bg-orange-500 text-white text-sm font-bold py-2 px-4 rounded"
                    >
                      Mua ngay
                    </button>
                    <button class="absolute bottom-4 right-4 text-gray-500">
                      <i class="far fa-heart"> </i>
                    </button>
                  </div>
                </div>
                <button
                  class="absolute right-0 bg-teal-600 text-white rounded-full p-2"
                >
                  <i class="fas fa-chevron-right"> </i>
                </button>
              </div>
              <div class="flex justify-center mt-4 space-x-2">
                <span class="w-2 h-2 bg-gray-400 rounded-full"> </span>
                <span class="w-2 h-2 bg-teal-600 rounded-full"> </span>
                <span class="w-2 h-2 bg-gray-400 rounded-full"> </span>
                <span class="w-2 h-2 bg-gray-400 rounded-full"> </span>
              </div>
            </div>
          </div>
        </div>
      </main>
    </div>
  </body>
</template>

<script setup>
import { useRoute } from "vue-router";
import { onMounted, ref } from "vue";
import api from "@/services/ApiService";
import { addToCart } from "@/services/CartService.js";
import { useToast } from "primevue/usetoast";
import getImageUrl from "@/utils/ImageUtils";
import { formatCurrency } from "@/utils/formatters";

const route = useRoute();
const toast = useToast();
const detailProduct = ref(null);
const quantity = ref(1);
const selectedColor = ref(null);

const fetchProductDetail = async () => {
  try {
    const response = await api.get(`product/${route.params.id}`);
    detailProduct.value = response.data;
    detailProduct.value.images = JSON.parse(detailProduct.value.images);
    detailProduct.value.colors = JSON.parse(detailProduct.value.colors);
    detailProduct.value.attributes = JSON.parse(detailProduct.value.attributes);
  } catch (error) {
    console.error("Error fetching product details:", error);
  }
};

const handleAddToCart = () => {
  addToCart(detailProduct.value.id, quantity.value, selectedColor.value);
  toast.add({
    severity: "success",
    summary: "Thành công",
    detail: "Sản phẩm đã được thêm vào giỏ hàng",
    life: 3000,
  });
};

onMounted(async () => {
  await fetchProductDetail();
});
</script>
