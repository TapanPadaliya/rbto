<template>
  <div class="max-w-lg w-full my-12">
    <ClientOnly>
      <Vueform v-model="formData" @submit="submitForm" :endpoint="t => t" ref="form$">
        <TextElement name="username" label="Username" :rules="['min:5', 'alpha_num']" />
        <TextElement name="email" input-type="email" :rules="['nullable', 'required', 'email']" label="Email Address" />
        <CheckboxElement name="visible_email" text="Make Email Visible For Public" />
        <TextElement name="member_name" label="Name Of Member" />
        <FileElement name="profile_pic" label="Member Photo" accept="image" view="image"
          :rules="['required', 'mimetypes:image/jpeg,image/png,image/webp']" :soft-remove="true" :drop="true"
          @input="onFileChange" :upload-temp-endpoint="(f) => { return f }" />
        <TextElement name="membership_no" label="Membership No." input-type="number" :rules="['required']" />
        <TextElement name="mobile_number" input-type="number" :rules="['nullable', 'required', 'numeric']"
          autocomplete="off" label="Mobile No." />
        <DateElement name="date_of_birth" label="Date Of Birth" :rules="['required']" />
        <TextElement name="spouse_name" label="Spouse Name" :rules="['required']" />
        <TextElement name="spouse_mobile_no" input-type="number" :rules="['nullable', 'required', 'numeric']"
          autocomplete="off" label="Spouse Mobile No." />
        <TextElement name="spouse_email" input-type="email" :rules="['nullable', 'required', 'email']"
          label="Spouse Email Address" />
        <DateElement name="spouse_dob" label="Spouse Date Of Birth" :rules="['required']" />
        <DateElement name="date_of_anniversary" label="Date Of Anniversary" :rules="['required']" />
        <TextareaElement name="residence_address" label="Residence Address" :rules="['required']" />
        <TextElement name="business_category" label="Business Category" :rules="['required']" />
        <TextElement name="business_name" label="Business Name" :rules="['required']" />
        <FileElement name="business_logo" label="Business Logo" accept="image" view="image"
          :rules="['required', 'mimetypes:image/jpeg,image/png,image/webp']" :soft-remove="true" :drop="true"
          @input="onFileChangeLogo" :upload-temp-endpoint="(f) => { return f }" />
        <TextareaElement name="business_address" label="Business Address" :rules="['required']" />
        <TextElement name="interest_hobbies" label="Interest Hobbies" :rules="['required']" />
        <TextElement name="password" input-type="password" label="Password" :rules="['required', 'min:8', 'max:36']" />
        <TextElement name="password_confirmation" input-type="password" label="Confirm Password"
          :rules="['required', 'same:password']" />
        <CheckboxElement name="terms_conditions" text="accept terms and conditions" :rules="['accepted']" />
        <div class="vf-col-12 flex items-center justify-center my-4">
          <button type="submit" class="px-4 py-2 border rounded-xl border-black hover:bg-gray-100">
            Submit
          </button>
        </div>
      </Vueform>
    </ClientOnly>
  </div>
</template>
<script setup>
import { ref } from 'vue';
import axios from 'axios';
const { $toast } = useNuxtApp();

const baseURL = 'http://localhost:8000/';
const formData = ref({});
const profile_pic_base64 = ref();
const business_logo_base64 = ref();
const form$ = ref(null)

const axiosInstance = axios.create({
  baseURL,
  headers: {
    'store': 'rbto',
    'ngrok-skip-browser-warning': 'fjsdhfjsdhfsdjf',
  },
});

const submitForm = async () => {
  const preparedData = formData.value;
  preparedData.profile_pic = profile_pic_base64.value
  preparedData.business_logo = business_logo_base64.value
  delete preparedData.terms_conditions

  try {
    const response = await axiosInstance.post('/auth/register', preparedData);
    if (response?.data && response.data?.statusCode == 1) {
      $toast.success("User registered successfully.");
      formData.value = {}
      form$.value.reset(); // Reset form data
      form$.value.clearValidation(); // Clear validation
      form$.value.clean()
      profile_pic_base64.value = null
      business_logo_base64.value = null
    }

  } catch (error) {
    if (error?.AxiosError?.message) {
      $toast.error(error.AxiosError.message);
    }
  }
};

const onFileChange = (event) => {
  const file = event.target.files[0];
  const reader = new FileReader();
  reader.readAsDataURL(file);
  reader.onload = () => {
    const base64Image = reader.result;
    profile_pic_base64.value = base64Image;
  };
};
const onFileChangeLogo = (event) => {
  const file = event.target.files[0];
  const reader = new FileReader();
  reader.readAsDataURL(file);
  reader.onload = () => {
    const base64Image = reader.result;
    business_logo_base64.value = base64Image;
  };
};
</script>
