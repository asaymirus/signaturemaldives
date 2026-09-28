<template>
  <div class="gmail-integration">
    <h3>Gmail Integration</h3>
    <p class="desc">Easily add your ITD Signature to Gmail in just a few steps.</p>

    <el-steps
      :active="activeStep"
      :process-status="processStatus"
      align-center
    >
      <el-step title="Copy Signature" />
      <el-step title="Open Gmail Settings" />
      <el-step title="Paste Signature" />
      <el-step title="Complete" />
    </el-steps>

    <div class="content">
      <div
        v-if="activeStep === 0"
        class="step-content"
      >
        <h4>Step 1: Copy Your Signature</h4>
        <p>Click the button below to copy your signature as HTML. This will copy the exact code needed for Gmail.</p>
        <el-button
          type="primary"
          size="large"
          @click="copySignatureForGmail"
        >
          <i class="el-icon-document-copy" /> Copy Signature HTML
        </el-button>
        <div class="success-message" v-if="copySuccess">
          <i class="el-icon-success" /> Signature copied to clipboard!
        </div>
      </div>

      <div
        v-if="activeStep === 1"
        class="step-content"
      >
        <h4>Step 2: Open Gmail Settings</h4>
        <ol>
          <li>Go to <strong>Gmail.com</strong></li>
          <li>Click the <strong>Gear Icon</strong> in the top right</li>
          <li>Select <strong>Settings</strong></li>
          <li>Go to the <strong>Forwarding and POP/IMAP</strong> tab</li>
          <li>Scroll down to <strong>Signature</strong> section</li>
        </ol>
        <el-button
          type="primary"
          @click="openGmailSettings"
        >
          <i class="el-icon-link" /> Open Gmail Settings
        </el-button>
      </div>

      <div
        v-if="activeStep === 2"
        class="step-content"
      >
        <h4>Step 3: Paste Your Signature</h4>
        <ol>
          <li>In Gmail Settings, find the <strong>Signature</strong> section</li>
          <li>Click in the text area</li>
          <li>Press <strong>Ctrl+V (or Cmd+V on Mac)</strong> to paste</li>
          <li>Make sure to select this signature for your email account</li>
          <li>Click <strong>Save Changes</strong> at the bottom</li>
        </ol>
        <div class="image-preview">
          <img src="@/assets/image/gmail-signature-guide.png" alt="Gmail Settings" v-if="false">
          <p style="color: #999; text-align: center;">
            Visual guide coming soon
          </p>
        </div>
      </div>

      <div
        v-if="activeStep === 3"
        class="step-content"
      >
        <h4>✅ All Done!</h4>
        <p>Your ITD Signature has been successfully added to Gmail!</p>
        <p>Your signature will now appear automatically on all new emails you compose.</p>
        <div class="info-box">
          <p><strong>Tips:</strong></p>
          <ul>
            <li>You can create multiple signatures and switch between them</li>
            <li>The signature appears automatically on new emails but not on replies</li>
            <li>You can edit your signature anytime in Gmail Settings</li>
            <li>Remember to save your changes before leaving Settings</li>
          </ul>
        </div>
      </div>
    </div>

    <div class="navigation-buttons">
      <el-button
        :disabled="activeStep === 0"
        @click="previousStep"
      >
        <i class="el-icon-arrow-left" /> Previous
      </el-button>
      <el-button
        :disabled="activeStep === 3"
        type="primary"
        @click="nextStep"
      >
        Next <i class="el-icon-arrow-right" />
      </el-button>
    </div>

    <!-- Hidden textarea for copy functionality -->
    <textarea
      ref="gmailHtml"
      v-model="signatureHtml"
      style="opacity: 0"
    />
  </div>
</template>

<script>
export default {
  name: 'Gmail',

  data () {
    return {
      activeStep: 0,
      processStatus: 'success',
      copySuccess: false,
      signatureHtml: ''
    }
  },

  created () {
    this.$ga.page(this.$router)
  },

  methods: {
    copySignatureForGmail () {
      // Get the signature from the template in Preview component
      const preview = document.querySelector('.email-preview > div')
      if (preview) {
        this.signatureHtml = preview.outerHTML.replace(/<!---->/g, '')
        setTimeout(() => {
          this.$refs.gmailHtml.select()
          document.execCommand('copy')
          this.copySuccess = true
          this.$message.success('Signature copied! Ready to paste in Gmail.')
          setTimeout(() => {
            this.copySuccess = false
          }, 3000)
        }, 10)
      }
    },
    openGmailSettings () {
      window.open('https://mail.google.com/mail/u/0/#settings', '_blank')
    },
    nextStep () {
      if (this.activeStep < 3) {
        this.activeStep++
      }
    },
    previousStep () {
      if (this.activeStep > 0) {
        this.activeStep--
      }
    }
  }
}
</script>

<style lang="scss">
.gmail-integration {
  padding: 20px 0;

  h3 {
    margin-bottom: 10px;
  }

  .desc {
    color: #999;
    margin-bottom: 30px;
    font-size: 14px;
  }

  .content {
    background: #f9f9f9;
    border: 1px solid #e0e0e0;
    border-radius: 4px;
    padding: 30px;
    margin-top: 30px;
    min-height: 300px;
  }

  .step-content {
    animation: fadeIn 0.3s ease-in;

    h4 {
      color: #333;
      font-size: 18px;
      margin-bottom: 15px;
    }

    p {
      color: #666;
      line-height: 1.6;
      margin-bottom: 15px;
    }

    ol {
      margin-left: 20px;
      margin-bottom: 15px;

      li {
        color: #666;
        line-height: 1.8;
        margin-bottom: 8px;

        strong {
          color: #333;
        }
      }
    }

    .image-preview {
      text-align: center;
      margin: 30px 0;
      padding: 20px;
      background: #fff;
      border-radius: 4px;

      img {
        max-width: 100%;
        height: auto;
      }
    }

    .success-message {
      background-color: #f0f9ff;
      border-left: 4px solid #67c23a;
      padding: 15px;
      margin-top: 20px;
      border-radius: 4px;
      color: #67c23a;
      font-weight: 500;
    }

    .info-box {
      background: #e6f7ff;
      border-left: 4px solid #409eff;
      padding: 15px;
      margin-top: 20px;
      border-radius: 4px;

      p {
        color: #409eff;
        margin: 0 0 10px 0;

        strong {
          color: #0066cc;
        }
      }

      ul {
        margin-left: 20px;
        padding: 0;
        color: #666;

        li {
          margin-bottom: 8px;
          line-height: 1.6;
        }
      }
    }
  }

  .navigation-buttons {
    margin-top: 30px;
    text-align: center;

    button {
      margin: 0 10px;
    }
  }
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
</style>
