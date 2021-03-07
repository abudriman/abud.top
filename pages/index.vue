<template>
  <div>
    <section class="hero is-medium is-info">
      <div class="hero-body">
        <div class="container">
          <div class="columns is-vcentered reverse-row-order">
            <div class="column">
              <div class="is-block">
                <p class="title is-1 has-text-weight-bold">
                  Simple URL Shortener
                </p>
                <p class="subtitle is-2 py-3">Shorten. Custom. Save.</p>
              </div>
            </div>
            <div class="column index-hero-svg">
              <img src="~assets/svg/undraw_link_shortener_mvf6.svg" />
            </div>
          </div>
        </div>
      </div>
    </section>
    <section class="hero is-small is-primary">
      <div class="hero-body">
        <div class="container">
          <div class="columns is-tablet">
            <div class="column">
              <input
                v-model.trim="$v.longURL.$model"
                class="input"
                type="text"
                name="longURL"
                autocomplete="off"
                placeholder="Place your URL here"
              />
            </div>
            <div class="column is-4">
              <button class="button is-fullwidth is-link">Shorten</button>
            </div>
          </div>
          <div class="columns is-tablet">
            <div v-show="$v.$dirty" class="column py-0">
              <div v-show="!$v.longURL.required" class="error">
                <span class="has-text-danger has-background-white px-1"
                  >Error</span
                >
                Field is required
              </div>
              <div v-show="!$v.longURL.url" class="error">
                <span class="has-text-danger has-background-white px-1"
                  >Error</span
                >
                Please enter valid URL address and make sure it starts with
                http:// or https://
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
    <section v-show="showResult" id="result" class="section result">
      <div class="container">
        <div class="columns">
          <div class="column is-fullwidth index-result-close">
            <div class="delete is-large"></div>
          </div>
        </div>
        <h3 class="title is-3">Here you go!</h3>
        <div class="columns">
          <div class="column pb-0">
            <h6 class="title is-6 mb-1 is-block">Shortened version:</h6>
            <div class="notification is-link is-light">
              <div class="columns is-vcentered">
                <div class="column index-short-link">
                  {{ shortURL }}
                  <span>
                    <a @click.prevent="editShortURL"
                      ><b-icon
                        icon="pencil"
                        size="is-small"
                        type="is-link" /></a
                  ></span>
                </div>
                <div class="column is-one-fifth">
                  <button class="button is-info is-fullwidth" disabled>
                    Copy
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="columns on-edit">
          <div class="column py-0">
            <p class="is-size-6">
              <span class="has-text-danger">Error:</span> URL already exist,
              please try another one.
            </p>
          </div>
        </div>
        <div class="columns on-edit">
          <div class="column pt-0">
            <button class="button is-primary">Save</button>
            <button class="button is-danger">Cancel</button>
          </div>
        </div>
        <h6 class="title is-6 mb-1">Unshortened URL:</h6>
        <div class="box">
          <div class="columns is-vcentered">
            <div class="column">
              <div class="index-raw-link">
                <h2>
                  {{ LongURL }}
                </h2>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <section class="section has-background-dark">
      <div class="container">
        <div v-if="authenticated" class="columns is-centered">
          <div class="column is-half has-text-centered">
            <div class="is-block">
              <p class="title is-1 has-text-white">Your History</p>
            </div>
            <div class="is-block">
              <p class="subtitle is-7 has-text-white">
                These URL list is private, not shown on public history
              </p>
            </div>
          </div>
        </div>
        <div v-else class="columns is-centered">
          <div class="column is-half has-text-centered">
            <div class="is-block">
              <p class="title is-1 has-text-white">Public History</p>
            </div>
            <div class="is-block">
              <p class="subtitle is-7 has-text-white">
                Log in to see your private url list.
              </p>
            </div>
          </div>
        </div>
        <div class="columns is-centered">
          <div class="column is-fullwidth index-table-histor table-container">
            <table class="table is-fullwidth index-table">
              <thead>
                <tr>
                  <th>Unshortened</th>
                  <th>Shortened</th>
                  <td>Action</td>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td class="index-td">
                    https://drive.google.com/file/dhttps://drive.google.com/file/dhttps://drive.google.com/file/dhttps://drive.google.com/file/dhttps://drive.google.com/file/dhttps://drive.google.com/file/dhttps://drive.google.com/file/dhttps://drive.google.com/file/d
                  </td>
                  <td>https://drive.google.com/file/d</td>
                  <td>
                    <div class="columns">
                      <div class="column">
                        <button class="button is-warning is-small">Edit</button>
                        <button class="button is-danger is-small">
                          Delete
                        </button>
                      </div>
                    </div>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>

          <div class="column is-half has-text-centered is-hidden">
            <p class="title is-7 has-text-white">
              You haven't shortened a URL. Try paste a URL inside above text box
              and click shorten.
            </p>
          </div>
        </div>
        <div v-if="authenticated" class="columns is-centered">
          <div class="column is-half has-text-centered">
            <button class="button is-primary">Switch to public history</button>
          </div>
        </div>
      </div>
    </section>
    <div class="modal" :class="{ 'is-active': isModalOpen }">
      <div class="modal-background" @click="toggleModalSignIn"></div>
      <div class="modal-content">
        <section class="section">
          <div class="container">
            <div v-if="isSignIn" class="columns is-centered">
              <div
                class="column is-one-third-desktop is-half-tablet has-background-white"
              >
                <div class="columns is-centered">
                  <div class="column is-flex is-justify-content-center">
                    <figure class="has-background-white image is-128x128">
                      <img src="~assets/svg/abudriman-logo-only.svg" />
                    </figure>
                  </div>
                </div>
                <div class="columns">
                  <div class="column is-flex is-flex-direction-column">
                    <h3 class="title is-3">Sign in</h3>
                    <a class="is-align-self-flex-end" @click="toggleModalMode"
                      >Create an account instead?</a
                    >
                  </div>
                </div>
                <div class="columns">
                  <div class="column">
                    <div class="field">
                      <p class="control has-icons-left has-icons-right">
                        <input class="input" type="email" placeholder="Email" />
                        <span class="icon is-small is-left">
                          <b-icon icon="email" size="is-small" />
                        </span>
                      </p>
                    </div>
                  </div>
                </div>
                <div class="columns">
                  <div class="column">
                    <div class="field">
                      <p class="control has-icons-left has-icons-right">
                        <input
                          class="input"
                          type="password"
                          placeholder="Password"
                        />
                        <span class="icon is-small is-left">
                          <b-icon icon="lock" size="is-small" />
                        </span>
                      </p>
                    </div>
                  </div>
                </div>
                <div class="columns">
                  <div class="column py-0">
                    <a class="">Forgot your password?</a>
                  </div>
                </div>
                <div class="columns">
                  <div class="column">
                    <button class="button is-primary is-fullwidth">
                      Sign in
                    </button>
                  </div>
                </div>
                <div class="columns is-centered">
                  <div class="column has-text-centered">
                    <p class="has-text-weight-bold">OR</p>
                  </div>
                </div>
                <div class="columns">
                  <div class="column">
                    <button class="button is-fullwidth is-info">
                      <figure class="has-background-white image is-24x24 mr-3">
                        <img src="~assets/svg/google-icon.svg" />
                      </figure>

                      Sign in with Google
                    </button>
                  </div>
                </div>
              </div>
            </div>
            <div v-else class="columns is-centered">
              <div class="column is-one-third has-background-white">
                <div class="columns">
                  <div class="column is-flex is-flex-direction-column">
                    <h3 class="title is-3">Sign Up</h3>
                    <a class="is-align-self-flex-end" @click="toggleModalMode"
                      >Already have an account?</a
                    >
                  </div>
                </div>
                <div class="columns">
                  <div class="column">
                    <div class="field">
                      <p class="control has-icons-left has-icons-right">
                        <input class="input" type="email" placeholder="Email" />
                        <span class="icon is-small is-left">
                          <b-icon icon="email" size="is-small" />
                        </span>
                      </p>
                    </div>
                  </div>
                </div>
                <div class="columns">
                  <div class="column">
                    <div class="field">
                      <p class="control has-icons-left has-icons-right">
                        <input
                          class="input"
                          type="password"
                          placeholder="Password"
                        />
                        <span class="icon is-small is-left">
                          <b-icon icon="lock" size="is-small" />
                        </span>
                      </p>
                    </div>
                  </div>
                </div>
                <div class="columns">
                  <div class="column">
                    <div class="field">
                      <p class="control has-icons-left has-icons-right">
                        <input
                          class="input"
                          type="password"
                          placeholder="Confirm Password"
                        />
                        <span class="icon is-small is-left">
                          <b-icon icon="lock" size="is-small" />
                        </span>
                      </p>
                    </div>
                  </div>
                </div>
                <div class="columns">
                  <div class="column">
                    <button class="button is-primary is-fullwidth">
                      Sign up
                    </button>
                  </div>
                </div>
                <div class="columns is-centered">
                  <div class="column has-text-centered">
                    <p class="has-text-weight-bold">OR</p>
                  </div>
                </div>
                <div class="columns">
                  <div class="column">
                    <button class="button is-fullwidth is-info">
                      <figure class="has-background-white image is-24x24 mr-3">
                        <img src="~assets/svg/google-icon.svg" />
                      </figure>

                      Sign up with Google
                    </button>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </section>
      </div>
      <button
        class="modal-close is-large"
        aria-label="close"
        @click="toggleModalSignIn"
      ></button>
    </div>
    <footer class="footer">
      <div class="content has-text-centered">
        <p>
          <strong>URL Shortener</strong> by
          <a href="https://journal.abud.top">Arif Budiman</a>
          <b-icon icon="link-variant" size="is-small" type="is-link" />. Visit
          my online journal to see my other works.
        </p>
        <div class="is-flex is-justify-content-center pt-3">
          <img
            src="~/assets/image/made-with-bulma.png"
            class="bulma-badge"
            width="200px"
          />
        </div>
      </div>
    </footer>
  </div>
</template>

<script>
import { required, url } from 'vuelidate/lib/validators'
import ShortUniqueId from 'short-unique-id'
const html = document.querySelector('html')
const uid = new ShortUniqueId()
export default {
  name: 'HomePage',
  data() {
    return {
      isModalOpen: false,
      isSignIn: true,
      longURL: '',
      shortURL: '',
      uuid: '',
      showResult: false,
      authenticated: false,
    }
  },
  validations: {
    longURL: {
      required,
      url,
    },
  },
  head() {
    return {
      title: 'abud.top | URL Shortener by Arif Budiman',
      meta: [
        // hid is used as unique identifier. Do not use `vmid` for it as it will not work
        {
          hid: 'abud.top',
          name: 'abud.top',
          content: 'URL Shortener by Arif Budiman',
        },
      ],
    }
  },
  created() {
    this.$nuxt.$on('open-modal', (value) => {
      this.toggleModalSignIn(value)
    })
    this.uuid = uid()
  },
  methods: {
    editShortURL() {
      return 0
    },
    toggleModalSignIn(mode) {
      this.isModalOpen = !this.isModalOpen
      html.classList.toggle('is-clipped')
      if (mode) {
        this.isSignIn = true
      } else {
        this.isSignIn = false
      }
    },
    toggleModalMode() {
      this.isSignIn = !this.isSignIn
    },
  },
}
</script>

<style lang="scss" scoped>
.index-raw-link,
.index-short-link {
  word-wrap: break-word;
  word-break: break-all;
}
.index-table-history {
  display: flex;
  justify-content: center;
  max-height: 600px;
  overflow-y: scroll;
}
.index-td {
  word-wrap: break-word;
  word-break: break-all;
}
.index-result-close {
  display: flex;
  justify-content: flex-end;
}
.index-table-history::-webkit-scrollbar {
  display: none;
}

/* Hide scrollbar for IE, Edge and Firefox */
.index-table-history {
  -ms-overflow-style: none; /* IE and Edge */
  scrollbar-width: none; /* Firefox */
}
@media (max-width: 768px) {
  .reverse-row-order {
    display: flex;
    flex-direction: column;
  }
  .index-hero-svg {
    order: -1;
  }
}
</style>
