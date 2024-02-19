<template>
  <div class="max-w-md">
    <Vueform>
      <TextElement
        name="username"
        label="Username"
        :rules="['min:5', 'alpha_num']"
      />
      <TextElement
        name="email"
        input-type="email"
        :rules="['nullable', 'required', 'email']"
        label="Email Address"
      />
      <CheckboxElement
        name="visible_email"
        text="Make Email Visible For Public"
      />
      <TextElement name="text_1" label="Name Of Member" />
      <FileElement
        name="profile_pic"
        label="Member Photo"
        accept="image/*"
        view="image"
        :rules="['required', 'mimetypes:image/jpeg,image/png,image/webp']"
        :soft-remove="true"
        :drop="true"
        @change="handleFileChange"
      />
      <TextElement
        name="membership_no"
        label="Membership No."
        input-type="number"
        :rules="['required']"
      />
      <TextElement
        name="mobile_number"
        input-type="number"
        :rules="['nullable', 'required', 'numeric']"
        autocomplete="off"
        label="Mobile No."
      />
      <DateElement
        name="date_of_birth"
        label="Date Of Birth"
        :rules="['required']"
      />
      <TextElement
        name="spouse_name"
        label="Spouse Name"
        :rules="['required']"
      />
      <TextElement
        name="spouse_mobile_no"
        input-type="number"
        :rules="['nullable', 'required', 'numeric']"
        autocomplete="off"
        label="Spouse Mobile No."
      />
      <TextElement
        name="spouse_email"
        input-type="email"
        :rules="['nullable', 'required', 'email']"
        label="Spouse Email Address"
      />
      <DateElement
        name="spouse_dob"
        label="Spouse Date Of Birth"
        :rules="['required']"
      />
      <DateElement
        name="date_of_anniversary"
        label="Date Of Anniversary"
        :rules="['required']"
      />
      <TextareaElement
        name="residence_address"
        label="Residence Address"
        :rules="['required']"
      />
      <TextElement
        name="business_category"
        label="Business Category"
        :rules="['required']"
      />
      <TextElement
        name="business_name"
        label="Business Name"
        :rules="['required']"
      />
      <TextareaElement
        name="business_address"
        label="Business Address"
        :rules="['required']"
      />
      <TextElement
        name="interest_hobbies"
        label="Interest Hobbies"
        :rules="['required']"
      />
      <TextElement
        name="password"
        input-type="password"
        label="Password"
        :rules="['required', 'min:8', 'max:36']"
      />
      <TextElement
        name="password_confirmation"
        input-type="password"
        label="Confirm Password"
        :rules="['required', 'same:password']"
      />
      <CheckboxElement
        name="terms_conditions"
        text="accept terms and conditions"
        :rules="['accepted']"
      />
      <div class="vf-col-12 flex items-center justify-center my-4">
        <button
          type="submit"
          class="px-4 py-2 border rounded-xl border-black hover:bg-gray-100"
        >
          Submit
        </button>
      </div>
    </Vueform>
  </div>
</template>
<script setup>
const handleFileChange = (file) => {
  const reader = new FileReader();
  reader.onload = () => {
    const base64Data = reader.result;
    // Use the base64Data as needed
    console.log(base64Data);
  };
  reader.readAsDataURL(file);
};
</script>
