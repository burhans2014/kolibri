<template>

  <KModal
    :title="coreString('deviceNameLabel')"
    :submitText="coreString('saveAction')"
    :cancelText="coreString('cancelAction')"
    @submit="handleSubmit"
    @cancel="$emit('cancel')"
  >
    <p>
      {{ $tr('deviceNameExplanation') }}
    </p>
    <KTextbox
      ref="name"
      v-model.trim="name"
      type="text"
      :label="coreString('deviceNameLabel')"
      :autofocus="true"
      :invalid="nameIsInvalid"
      :invalidText="nameIsInvalidText"
      :maxlength="50"
      @blur="nameBlurred = true"
    />
  </KModal>

</template>


<script>

  import commonCoreStrings from 'kolibri.coreVue.mixins.commonCoreStrings';

  export default {
    name: 'DeviceNameModal',
    mixins: [commonCoreStrings],
    props: {
      deviceName: {
        type: String,
        required: false,
      },
    },
    data() {
      return {
        name: this.deviceName,
        nameBlurred: false,
        formSubmitted: false,
      };
    },
    computed: {
      nameIsInvalidText() {
        if (this.nameBlurred || this.formSubmitted) {
          if (this.name === '') {
            return this.coreString('requiredFieldError');
          }
        }
        return '';
      },
      nameIsInvalid() {
        return Boolean(this.nameIsInvalidText);
      },
    },
    methods: {
      handleSubmit() {
        this.formSubmitted = true;
        if (this.nameIsInvalid) {
          this.$refs.name.focus();
        } else {
          this.$emit('submit', this.name);
        }
      },
    },
    $trs: {
      deviceNameExplanation: {
        message:
          'Giving this device a unique and meaningful name can help you and others in your network to recognize it',
        context: 'Explanation of what the device name is',
      },
    },
  };

</script>


<style lang="scss" scoped></style>
