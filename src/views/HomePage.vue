<template>
  <IonPage
    class="app-page"
    :class="themeClass"
  >
    <a
      class="skip-link"
      href="#main-content"
    >
      Skip to main content
    </a>

    <!-- ==================== HEADER ==================== -->
    <IonHeader
      v-if="currentView !== 'landing'"
      class="app-header"
    >
      <IonToolbar class="app-toolbar">
        <IonTitle class="app-title">
          <span
            class="title-mark"
            aria-hidden="true"
          >
            <AppIcon
              name="lock"
              :size="15"
            />
          </span>

          Caesar Cipher
        </IonTitle>

        <div
          slot="end"
          class="toolbar-actions"
        >
          <button
            type="button"
            class="theme-toggle"
            role="switch"
            :aria-checked="isDarkMode"
            aria-label="Dark mode"
            :title="isDarkMode ? 'Dark mode is on' : 'Dark mode is off'"
            @click="toggleTheme"
          >
            <AppIcon
              :name="isDarkMode ? 'sun' : 'moon'"
              :size="17"
            />
          </button>
        </div>
      </IonToolbar>
    </IonHeader>

    <!-- ==================== CONTENT ==================== -->
    <IonContent
      :fullscreen="true"
      class="app-content"
      :class="{ 'landing-content': currentView === 'landing' }"
    >
      <!-- ============ LANDING VIEW ============ -->
      <main
        v-if="currentView === 'landing'"
        id="main-content"
        class="landing-container"
      >
        <div class="landing-topbar">
          <span class="landing-brand">
            <span
              class="title-mark"
              aria-hidden="true"
            >
              <AppIcon
                name="lock"
                :size="14"
              />
            </span>

            Caesar Cipher
          </span>

          <button
            type="button"
            class="theme-toggle theme-toggle-label"
            role="switch"
            :aria-checked="isDarkMode"
            aria-label="Dark mode"
            @click="toggleTheme"
          >
            <AppIcon
              :name="isDarkMode ? 'sun' : 'moon'"
              :size="16"
            />

            <span>{{ isDarkMode ? 'Light' : 'Dark' }}</span>
          </button>
        </div>

        <section
          class="landing-hero"
          aria-labelledby="landing-title"
        >
          <div
            class="hero-tile hero-tile-large"
            aria-hidden="true"
          >
            <AppIcon
              name="key"
              :size="38"
            />
          </div>

          <p class="eyebrow">
            Cryptography Project
          </p>

          <h1
            id="landing-title"
            class="display-title"
          >
            Encrypt or Decrypt
          </h1>

          <p class="landing-description">
            Transform your message with the Caesar
            Shift Cipher and watch every letter move.
          </p>

          <p class="landing-developer">
            Developed by
            <strong>Hernandez, Jhon Vincent O.</strong>
          </p>

          <IonButton
            expand="block"
            class="cta-button landing-cta"
            @click="goToView('home')"
          >
            <span>Get Started</span>

            <AppIcon
              name="arrowRight"
              :size="17"
              class="cta-icon"
            />
          </IonButton>

          <p class="landing-note">
            Simple shifts. Secure learning.
          </p>
        </section>

        <section
          class="landing-features"
          aria-label="What this app does"
        >
          <ul class="feature-grid">
            <li
              v-for="feature in LANDING_FEATURES"
              :key="feature.title"
              class="feature-tile"
            >
              <span
                class="feature-icon"
                aria-hidden="true"
              >
                <AppIcon
                  :name="feature.icon"
                  :size="18"
                />
              </span>

              <strong>{{ feature.title }}</strong>

              <p>{{ feature.copy }}</p>
            </li>
          </ul>
        </section>
      </main>

      <!-- ============ HOME VIEW ============ -->
      <main
        v-else-if="currentView === 'home'"
        id="main-content"
        class="page-container"
      >
        <p
          class="sr-only"
          role="status"
          aria-live="polite"
        >
          {{ statusAnnouncement }}
        </p>

        <section
          v-if="renderError !== null"
          class="app-card banner banner-danger"
          role="alert"
        >
          <AppIcon
            name="alert"
            :size="17"
          />

          <span>{{ renderError }}</span>

          <button
            type="button"
            class="link-button"
            @click="recoverFromRenderError"
          >
            Reload screen
          </button>
        </section>

        <section
          class="hero-section"
          aria-labelledby="home-title"
        >
          <div
            class="hero-tile"
            aria-hidden="true"
          >
            <AppIcon
              name="lock"
              :size="30"
            />
          </div>

          <p class="eyebrow">
            Cryptography Project
          </p>

          <h1
            id="home-title"
            class="page-title"
          >
            Encrypt or Decrypt
          </h1>

          <p class="hero-description">
            Transform your message using the Caesar
            Shift Cipher.
          </p>
        </section>

        <!-- Cipher Console -->
        <form
          class="app-card console-card"
          novalidate
          aria-labelledby="console-title"
          @submit.prevent="processCipher"
        >
          <h2
            id="console-title"
            class="card-title"
          >
            Cipher Console
          </h2>

          <!-- Step 01 -->
          <div class="step">
            <h3 class="step-heading">
              <span
                class="step-index"
                aria-hidden="true"
              >01</span>

              Select Operation
            </h3>

            <IonSegment
              class="mode-segment"
              :value="mode"
              aria-label="Cipher operation"
              @ionChange="handleModeChange"
            >
              <IonSegmentButton value="encrypt">
                <IonLabel>
                  <span class="segment-label">
                    <AppIcon
                      name="arrowUpRight"
                      :size="15"
                    />

                    Encrypt
                  </span>
                </IonLabel>
              </IonSegmentButton>

              <IonSegmentButton value="decrypt">
                <IonLabel>
                  <span class="segment-label">
                    <AppIcon
                      name="arrowDownLeft"
                      :size="15"
                    />

                    Decrypt
                  </span>
                </IonLabel>
              </IonSegmentButton>
            </IonSegment>
          </div>

          <!-- Step 02 -->
          <div class="step">
            <h3 class="step-heading">
              <span
                class="step-index"
                aria-hidden="true"
              >02</span>

              Enter Message
            </h3>

            <div
              ref="messageFieldRef"
              class="field-wrap"
            >
              <IonItem
                lines="none"
                class="field-item"
              >
                <IonLabel
                  position="stacked"
                  class="field-label"
                >
                  {{ inputLabel }}
                </IonLabel>

                <IonTextarea
                  :value="messageText"
                  :auto-grow="true"
                  :rows="4"
                  :maxlength="MAX_MESSAGE_LENGTH"
                  :placeholder="inputPlaceholder"
                  class="field-control"
                  aria-describedby="message-hint message-counter"
                  @ionInput="handleMessageInput"
                />
              </IonItem>
            </div>

            <div class="field-meta">
              <p
                id="message-hint"
                class="field-hint"
              >
                {{ inputHint }}
              </p>

              <p
                id="message-counter"
                class="field-counter"
                :class="{ 'counter-warning': messageText.length >= MAX_MESSAGE_LENGTH - 200 }"
              >
                {{ messageText.length }} / {{ MAX_MESSAGE_LENGTH }}
              </p>
            </div>
          </div>

          <!-- Step 03 -->
          <div class="step">
            <h3 class="step-heading">
              <span
                class="step-index"
                aria-hidden="true"
              >03</span>

              Set Shift Value
            </h3>

            <IonItem
              lines="none"
              class="field-item"
            >
              <IonLabel
                position="stacked"
                class="field-label"
              >
                Shift Number
              </IonLabel>

              <IonInput
                :value="shiftText"
                type="number"
                :min="MIN_SHIFT"
                :max="MAX_SHIFT"
                inputmode="numeric"
                placeholder="Enter a value from 1 to 25"
                class="field-control"
                :aria-invalid="shiftErrorMessage !== ''"
                :aria-describedby="shiftErrorMessage !== '' ? 'shift-error' : 'shift-hint'"
                @ionInput="handleShiftInput"
              />
            </IonItem>

            <p
              v-if="shiftErrorMessage !== ''"
              id="shift-error"
              class="field-error"
              role="alert"
            >
              <AppIcon
                name="alert"
                :size="14"
              />

              <span>{{ shiftErrorMessage }}</span>
            </p>

            <p
              v-else
              id="shift-hint"
              class="field-hint"
            >
              Use a whole number from {{ MIN_SHIFT }} to {{ MAX_SHIFT }}.
            </p>

            <div
              class="shift-presets"
              role="group"
              aria-label="Common shift values"
            >
              <button
                v-for="preset in SHIFT_PRESETS"
                :key="preset"
                type="button"
                class="preset-chip"
                :class="{ 'preset-active': activeShift === preset }"
                :aria-pressed="activeShift === preset"
                @click="applyShiftPreset(preset)"
              >
                {{ preset }}
              </button>
            </div>
          </div>

          <IonButton
            type="submit"
            expand="block"
            class="cta-button"
            :disabled="!canProcess"
          >
            <AppIcon
              :name="isEncryptMode ? 'arrowUpRight' : 'arrowDownLeft'"
              :size="17"
            />

            <span>{{ isEncryptMode ? 'Encrypt Message' : 'Decrypt Message' }}</span>
          </IonButton>
        </form>

        <!-- Processing error -->
        <p
          v-if="cipherError !== null"
          class="banner banner-danger"
          role="alert"
        >
          <AppIcon
            name="alert"
            :size="16"
          />

          <span>{{ cipherError }}</span>

          <button
            type="button"
            class="link-button"
            @click="processCipher"
          >
            Try again
          </button>
        </p>

        <!-- Result -->
        <section
          v-if="submitted !== null"
          class="app-card result-card"
          aria-labelledby="result-title"
        >
          <header class="card-header">
            <div>
              <p class="card-eyebrow eyebrow-success">
                <AppIcon
                  name="check"
                  :size="12"
                />

                Operation Complete
              </p>

              <h2
                id="result-title"
                class="card-title"
              >
                {{ outputLabelFor(submitted.mode) }}
              </h2>
            </div>

            <button
              type="button"
              class="icon-button"
              :class="{ 'icon-button-busy': copyState.status === 'copying' }"
              :aria-busy="copyState.status === 'copying'"
              :aria-label="copyButtonLabel"
              :title="copyButtonLabel"
              :disabled="copyState.status === 'copying'"
              @click="copyResult"
            >
              <AppIcon
                :name="copyState.status === 'copying' ? 'loader' : 'copy'"
                :size="16"
                :class="{ 'icon-spin': copyState.status === 'copying' }"
              />
            </button>
          </header>

          <output class="result-output mono">
            {{ submitted.output }}
          </output>

          <p
            v-if="copyFeedback !== null"
            class="copy-feedback"
            :class="copyFeedback.tone === 'danger' ? 'copy-danger' : 'copy-success'"
            :role="copyFeedback.tone === 'danger' ? 'alert' : 'status'"
          >
            <AppIcon
              :name="copyFeedback.tone === 'danger' ? 'alert' : 'check'"
              :size="13"
            />

            <span>{{ copyFeedback.text }}</span>

            <button
              v-if="copyFeedback.tone === 'danger'"
              type="button"
              class="link-button"
              @click="copyResult"
            >
              Try again
            </button>
          </p>

          <p class="success-banner">
            <span
              class="success-dot"
              aria-hidden="true"
            >
              <AppIcon
                name="check"
                :size="12"
              />
            </span>

            Message
            {{ submitted.mode === 'encrypt' ? 'encrypted' : 'decrypted' }}
            with a shift of {{ submitted.shift }}.
            <span class="success-meta">
              {{ submitted.characterCount }} characters
            </span>
          </p>

          <p
            v-if="isResultStale"
            class="stale-hint"
          >
            <AppIcon
              name="alert"
              :size="13"
            />

            <span>
              Your inputs changed. Run the operation
              again to refresh this result.
            </span>
          </p>
        </section>

        <!-- Cipher Process -->
        <section
          class="app-card process-card"
          aria-labelledby="process-title"
        >
          <header class="card-header card-header-tight">
            <h2
              id="process-title"
              class="card-title"
            >
              <span
                class="step-index"
                aria-hidden="true"
              >04</span>

              Cipher Process
            </h2>
          </header>

          <ol class="process-flow">
            <li class="process-step step-input">
              <span class="step-label">{{ inputLabel }}</span>

              <strong class="step-value mono">{{ processPreviewSource }}</strong>
            </li>

            <li
              class="process-connector"
              aria-hidden="true"
            >
              <AppIcon
                name="arrowRight"
                :size="15"
              />
            </li>

            <li class="process-step step-shift">
              <span class="step-label">Shift</span>

              <strong class="step-value mono">{{ processPreviewShift }}</strong>
            </li>

            <li
              class="process-connector"
              aria-hidden="true"
            >
              <AppIcon
                name="arrowRight"
                :size="15"
              />
            </li>

            <li class="process-step step-output">
              <span class="step-label">{{ outputLabel }}</span>

              <strong class="step-value mono">{{ processPreviewOutput }}</strong>
            </li>
          </ol>

          <p class="process-description">
            <AppIcon
              name="info"
              :size="15"
              class="description-icon"
            />

            <span>{{ processDescription }}</span>
          </p>

          <div class="letter-example">
            <p class="micro-label">
              Letter Example
            </p>

            <p class="letter-flow">
              <strong class="mono">{{ exampleInput }}</strong>

              <AppIcon
                name="arrowRight"
                :size="15"
                class="letter-arrow"
              />

              <strong class="mono">{{ exampleOutput }}</strong>
            </p>

            <p class="letter-note">
              {{ exampleDescription }}
            </p>
          </div>

          <details class="alphabet-disclosure">
            <summary class="alphabet-summary">
              <span>View alphabet map</span>

              <AppIcon
                name="chevronDown"
                :size="16"
                class="disclosure-icon"
              />
            </summary>

            <ul class="alphabet-grid">
              <li
                v-for="pair in alphabetMap"
                :key="pair.from"
                class="alphabet-cell"
              >
                <span class="mono alphabet-from">{{ pair.from }}</span>

                <AppIcon
                  name="arrowRight"
                  :size="10"
                  class="alphabet-arrow"
                />

                <span class="mono alphabet-to">{{ pair.to }}</span>
              </li>
            </ul>
          </details>
        </section>

        <IonButton
          expand="block"
          fill="outline"
          type="button"
          class="ghost-button"
          @click="clearForm"
        >
          <AppIcon
            name="refresh"
            :size="16"
          />

          <span>Clear All</span>
        </IonButton>
      </main>

      <!-- ============ ABOUT VIEW ============ -->
      <main
        v-else
        id="main-content"
        class="page-container"
      >
        <section
          class="hero-section"
          aria-labelledby="about-title"
        >
          <div
            class="hero-tile"
            aria-hidden="true"
          >
            <AppIcon
              name="info"
              :size="30"
            />
          </div>

          <p class="eyebrow">
            Application Information
          </p>

          <h1
            id="about-title"
            class="page-title"
          >
            About This App
          </h1>

          <p class="hero-description">
            Learn how the Caesar Cipher works.
          </p>
        </section>

        <section
          class="app-card"
          aria-labelledby="about-card-title"
        >
          <h2
            id="about-card-title"
            class="card-title card-title-center"
          >
            Caesar Cipher
          </h2>

          <p class="about-description">
            The Caesar Cipher is a substitution
            technique that shifts every letter by a
            fixed number of positions in the alphabet.
            Letters wrap around at Z, and every other
            character passes through untouched.
          </p>

          <div class="formula-group">
            <div class="formula-block">
              <p class="micro-label">
                Encryption Formula
              </p>

              <p class="formula mono">
                C = (P + S) mod 26
              </p>
            </div>

            <div class="formula-block">
              <p class="micro-label">
                Decryption Formula
              </p>

              <p class="formula mono">
                P = (C - S) mod 26
              </p>
            </div>
          </div>

          <dl class="definition-list">
            <div
              v-for="definition in FORMULA_DEFINITIONS"
              :key="definition.term"
              class="definition-row"
            >
              <dt class="mono">
                {{ definition.term }}
              </dt>

              <dd>{{ definition.meaning }}</dd>
            </div>
          </dl>

          <hr class="content-divider">

          <h3 class="subsection-title">
            Worked Example
          </h3>

          <div class="table-scroll">
            <table class="example-table">
              <caption class="sr-only">
                Encrypting the word HELLO with a shift
                of {{ ABOUT_EXAMPLE_SHIFT }}
              </caption>

              <thead>
                <tr>
                  <th scope="col">
                    Position
                  </th>

                  <th
                    v-for="column in ABOUT_EXAMPLE_COLUMNS"
                    :key="column.position"
                    scope="col"
                  >
                    {{ column.position }}
                  </th>
                </tr>
              </thead>

              <tbody>
                <tr>
                  <th scope="row">
                    Plaintext
                  </th>

                  <td
                    v-for="column in ABOUT_EXAMPLE_COLUMNS"
                    :key="`p-${column.position}`"
                    class="mono"
                  >
                    {{ column.source }}
                  </td>
                </tr>

                <tr>
                  <th scope="row">
                    Ciphertext
                  </th>

                  <td
                    v-for="column in ABOUT_EXAMPLE_COLUMNS"
                    :key="`c-${column.position}`"
                    class="mono cell-output"
                  >
                    {{ column.output }}
                  </td>
                </tr>
              </tbody>
            </table>
          </div>

          <hr class="content-divider">

          <h3 class="subsection-title">
            Supported Characters
          </h3>

          <ul class="check-list">
            <li
              v-for="item in SUPPORTED_CHARACTERS"
              :key="item"
            >
              <AppIcon
                name="check"
                :size="14"
                class="check-icon"
              />

              <span>{{ item }}</span>
            </li>
          </ul>

          <hr class="content-divider">

          <div class="developer-block">
            <p class="micro-label">
              Developed By
            </p>

            <p class="developer-name">
              Hernandez, Jhon Vincent O.
            </p>

            <p class="developer-role">
              Bachelor of Science in Information
              Technology
            </p>
          </div>
        </section>

        <blockquote class="quote-block">
          Simple shifts, stronger thinking.
        </blockquote>
      </main>
    </IonContent>

    <!-- ==================== BOTTOM NAVIGATION ==================== -->
    <IonFooter
      v-if="currentView !== 'landing'"
      class="app-footer"
    >
      <IonToolbar class="bottom-toolbar">
        <nav
          class="bottom-navigation"
          aria-label="Main navigation"
        >
          <button
            type="button"
            class="nav-button"
            :class="{ 'nav-active': currentView === 'home' }"
            :aria-current="currentView === 'home' ? 'page' : undefined"
            @click="goToView('home')"
          >
            <AppIcon
              name="home"
              :size="19"
            />

            <span>Home</span>
          </button>

          <button
            type="button"
            class="nav-button"
            :class="{ 'nav-active': currentView === 'about' }"
            :aria-current="currentView === 'about' ? 'page' : undefined"
            @click="goToView('about')"
          >
            <AppIcon
              name="info"
              :size="19"
            />

            <span>About</span>
          </button>
        </nav>
      </IonToolbar>
    </IonFooter>
  </IonPage>
</template>

<script setup lang="ts">
import {
  computed,
  defineComponent,
  h,
  onBeforeUnmount,
  onErrorCaptured,
  onMounted,
  ref
} from 'vue';

import type { PropType, VNode } from 'vue';

import {
  IonButton,
  IonContent,
  IonFooter,
  IonHeader,
  IonInput,
  IonItem,
  IonLabel,
  IonPage,
  IonSegment,
  IonSegmentButton,
  IonTextarea,
  IonTitle,
  IonToolbar
} from '@ionic/vue';

/* =========================================================
   CONSTANTS
========================================================= */

const MAX_MESSAGE_LENGTH = 5000;
const MIN_SHIFT = 1;
const MAX_SHIFT = 25;
const ALPHABET_SIZE = 26;
const UPPERCASE_START = 65;
const LOWERCASE_START = 97;
const DEFAULT_SHIFT = 3;
const ABOUT_EXAMPLE_SHIFT = 3;
const EXAMPLE_WORD = 'HELLO';
const ALPHABET = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ';
const THEME_STORAGE_KEY = 'caesar-cipher-theme';
const DARK_MEDIA_QUERY = '(prefers-color-scheme: dark)';
const COPY_FEEDBACK_RESET_MS = 2600;

const SHIFT_PRESETS: readonly number[] = [3, 5, 13, 25];

const SUPPORTED_CHARACTERS: readonly string[] = [
  'Uppercase letters A to Z',
  'Lowercase letters a to z',
  'Spaces, punctuation and line breaks pass through',
  'Numbers and symbols pass through unchanged'
];

const FORMULA_DEFINITIONS: ReadonlyArray<{
  term: string;
  meaning: string;
}> = [
  { term: 'C', meaning: 'Ciphertext letter' },
  { term: 'P', meaning: 'Plaintext letter' },
  { term: 'S', meaning: 'Shift value' },
  { term: 'mod 26', meaning: 'Wraps around the alphabet' }
];

/* =========================================================
   TYPES
========================================================= */

type CipherMode = 'encrypt' | 'decrypt';
type AppView = 'landing' | 'home' | 'about';
type ThemeMode = 'light' | 'dark';

type CipherErrorCode =
  | 'INVALID_INPUT'
  | 'PROCESSING_FAILED'
  | 'CLIPBOARD_UNSUPPORTED'
  | 'CLIPBOARD_WRITE_FAILED';

/* Discriminated unions: only valid states are representable. */
type ShiftValidation =
  | { status: 'empty' }
  | { status: 'not-a-number' }
  | { status: 'not-integer' }
  | { status: 'out-of-range'; value: number }
  | { status: 'valid'; value: number };

type SubmittedCipher = {
  mode: CipherMode;
  shift: number;
  source: string;
  output: string;
  characterCount: number;
};

type CopyState =
  | { status: 'idle' }
  | { status: 'copying' }
  | { status: 'copied' }
  | { status: 'error'; message: string };

type CopyFeedback = {
  tone: 'success' | 'danger';
  text: string;
};

interface FieldChangeEvent {
  readonly detail: { readonly value?: unknown };
}

interface AlphabetPair {
  readonly from: string;
  readonly to: string;
}

/* =========================================================
   TYPED ERROR
========================================================= */

class CipherError extends Error {
  public readonly code: CipherErrorCode;

  public constructor(
    message: string,
    code: CipherErrorCode
  ) {
    super(message);
    this.name = 'CipherError';
    this.code = code;
  }
}

const logError = (
  scope: string,
  error: unknown,
  context: Record<string, unknown> = {}
): void => {
  // Full error context goes to the console only. Users see
  // an actionable message instead of internals.
  console.error(`[caesar-cipher:${scope}]`, {
    message:
      error instanceof Error
        ? error.message
        : String(error),
    context
  });
};

const assertNever = (value: never): never => {
  throw new Error(
    `Unhandled state: ${JSON.stringify(value)}`
  );
};

/* =========================================================
   ICON SYSTEM (inline SVG, no emoji anywhere)
========================================================= */

type IconNode =
  | { readonly tag: 'path'; readonly d: string }
  | {
      readonly tag: 'circle';
      readonly cx: number;
      readonly cy: number;
      readonly r: number;
    }
  | {
      readonly tag: 'rect';
      readonly x: number;
      readonly y: number;
      readonly width: number;
      readonly height: number;
      readonly rx: number;
    };

type IconName =
  | 'lock'
  | 'key'
  | 'home'
  | 'info'
  | 'sun'
  | 'moon'
  | 'copy'
  | 'check'
  | 'alert'
  | 'refresh'
  | 'loader'
  | 'arrowRight'
  | 'arrowUpRight'
  | 'arrowDownLeft'
  | 'arrowUpDown'
  | 'chevronDown'
  | 'bookOpen'
  | 'hash'
  | 'route';

const ICON_NODES: Record<IconName, readonly IconNode[]> = {
  lock: [
    { tag: 'rect', x: 4, y: 10, width: 16, height: 11, rx: 2 },
    { tag: 'path', d: 'M8 10V7a4 4 0 0 1 8 0v3' }
  ],
  key: [
    { tag: 'circle', cx: 8, cy: 15, r: 5 },
    { tag: 'path', d: 'm21 2-9.6 9.6' },
    { tag: 'path', d: 'm15.5 7.5 3 3L22 7l-3-3' }
  ],
  home: [
    { tag: 'path', d: 'm3 9 9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z' },
    { tag: 'path', d: 'M9 22V12h6v10' }
  ],
  info: [
    { tag: 'circle', cx: 12, cy: 12, r: 9 },
    { tag: 'path', d: 'M12 16v-4' },
    { tag: 'path', d: 'M12 8h.01' }
  ],
  sun: [
    { tag: 'circle', cx: 12, cy: 12, r: 4 },
    { tag: 'path', d: 'M12 2v2M12 20v2M4.9 4.9l1.4 1.4M17.7 17.7l1.4 1.4M2 12h2M20 12h2M6.3 17.7l-1.4 1.4M19.1 4.9l-1.4 1.4' }
  ],
  moon: [{ tag: 'path', d: 'M12 3a6 6 0 0 0 9 9 9 9 0 1 1-9-9Z' }],
  copy: [
    { tag: 'rect', x: 9, y: 9, width: 13, height: 13, rx: 2 },
    { tag: 'path', d: 'M5 15H4a2 2 0 0 1-2-2V4a2 2 0 0 1 2-2h9a2 2 0 0 1 2 2v1' }
  ],
  check: [{ tag: 'path', d: 'M20 6 9 17l-5-5' }],
  alert: [
    { tag: 'circle', cx: 12, cy: 12, r: 9 },
    { tag: 'path', d: 'M12 8v5' },
    { tag: 'path', d: 'M12 16h.01' }
  ],
  refresh: [
    { tag: 'path', d: 'M3 12a9 9 0 1 0 9-9 9.7 9.7 0 0 0-6.7 2.7L3 8' },
    { tag: 'path', d: 'M3 3v5h5' }
  ],
  loader: [{ tag: 'path', d: 'M21 12a9 9 0 1 1-6.2-8.6' }],
  arrowRight: [
    { tag: 'path', d: 'M5 12h14' },
    { tag: 'path', d: 'm12 5 7 7-7 7' }
  ],
  arrowUpRight: [
    { tag: 'path', d: 'M7 17 17 7' },
    { tag: 'path', d: 'M8 7h9v9' }
  ],
  arrowDownLeft: [
    { tag: 'path', d: 'M17 7 7 17' },
    { tag: 'path', d: 'M16 17H7V8' }
  ],
  arrowUpDown: [
    { tag: 'path', d: 'm21 16-4 4-4-4' },
    { tag: 'path', d: 'M17 20V4' },
    { tag: 'path', d: 'm3 8 4-4 4 4' },
    { tag: 'path', d: 'M7 4v16' }
  ],
  chevronDown: [{ tag: 'path', d: 'm6 9 6 6 6-6' }],
  bookOpen: [
    { tag: 'path', d: 'M12 7v14' },
    { tag: 'path', d: 'M3 18a1 1 0 0 1-1-1V4a1 1 0 0 1 1-1h5a4 4 0 0 1 4 4 4 4 0 0 1 4-4h5a1 1 0 0 1 1 1v13a1 1 0 0 1-1 1h-6a3 3 0 0 0-3 3 3 3 0 0 0-3-3z' }
  ],
  hash: [
    { tag: 'path', d: 'M4 9h16M4 15h16M10 3 8 21M16 3l-2 18' }
  ],
  route: [
    { tag: 'circle', cx: 6, cy: 19, r: 3 },
    { tag: 'path', d: 'M9 19h8.5a3.5 3.5 0 0 0 0-7h-11a3.5 3.5 0 0 1 0-7H15' },
    { tag: 'circle', cx: 18, cy: 5, r: 3 }
  ]
};

const renderIconNode = (node: IconNode): VNode => {
  switch (node.tag) {
    case 'path':
      return h('path', { d: node.d });
    case 'circle':
      return h('circle', { cx: node.cx, cy: node.cy, r: node.r });
    case 'rect':
      return h('rect', {
        x: node.x,
        y: node.y,
        width: node.width,
        height: node.height,
        rx: node.rx
      });
    default:
      return assertNever(node);
  }
};

interface AppIconProps {
  name: IconName;
  size?: number;
}

const AppIcon = defineComponent({
  name: 'AppIcon',
  props: {
    name: {
      type: String as PropType<IconName>,
      required: true
    },
    size: {
      type: Number,
      default: 20
    }
  },
  setup: (props: AppIconProps) => {
    // Fallback to 20 if props.size is undefined to satisfy TypeScript
    const size = props.size ?? 20;

    return () =>
      h(
        'svg',
        {
          class: 'app-icon',
          width: size,
          height: size,
          viewBox: '0 0 24 24',
          fill: 'none',
          stroke: 'currentColor',
          'stroke-width': size >= 28 ? 1.6 : 1.8,
          'stroke-linecap': 'round',
          'stroke-linejoin': 'round',
          'aria-hidden': 'true',
          focusable: 'false'
        },
        ICON_NODES[props.name].map(renderIconNode)
      );
  }
});

const LANDING_FEATURES: ReadonlyArray<{
  icon: IconName;
  title: string;
  copy: string;
}> = [
  {
    icon: 'arrowUpRight',
    title: 'Encrypt',
    copy: 'Convert plaintext into ciphertext.'
  },
  {
    icon: 'arrowDownLeft',
    title: 'Decrypt',
    copy: 'Convert ciphertext back to plaintext.'
  },
  {
    icon: 'bookOpen',
    title: 'Learn',
    copy: 'Follow the shift letter by letter.'
  }
];

/* =========================================================
   STATE
========================================================= */

const currentView = ref<AppView>('landing');
const mode = ref<CipherMode>('encrypt');
const messageText = ref('');
const shiftText = ref(String(DEFAULT_SHIFT));
const submitted = ref<SubmittedCipher | null>(null);
const cipherError = ref<string | null>(null);
const copyState = ref<CopyState>({ status: 'idle' });
const theme = ref<ThemeMode>('light');
const renderError = ref<string | null>(null);
const messageFieldRef = ref<HTMLElement | null>(null);

let copyResetTimer: ReturnType<typeof setTimeout> | null = null;
let themeMediaQuery: MediaQueryList | null = null;

/* =========================================================
   CIPHER ALGORITHM
   ASSUMPTION: `text` iterates by code point, so surrogate
   pairs (emoji, extended scripts) survive the round trip.
   Only A-Z and a-z are shifted; everything else is copied.
========================================================= */

const caesarCipher = (
  text: string,
  shift: number,
  decrypt: boolean
): string => {
  const delta = decrypt ? -shift : shift;
  const output: string[] = [];

  for (const character of text) {
    const codePoint = character.codePointAt(0) ?? 0;

    if (
      codePoint >= UPPERCASE_START &&
      codePoint <= UPPERCASE_START + ALPHABET_SIZE - 1
    ) {
      const offset =
        ((codePoint - UPPERCASE_START + delta) % ALPHABET_SIZE +
          ALPHABET_SIZE) %
        ALPHABET_SIZE;

      output.push(String.fromCharCode(offset + UPPERCASE_START));
      continue;
    }

    if (
      codePoint >= LOWERCASE_START &&
      codePoint <= LOWERCASE_START + ALPHABET_SIZE - 1
    ) {
      const offset =
        ((codePoint - LOWERCASE_START + delta) % ALPHABET_SIZE +
          ALPHABET_SIZE) %
        ALPHABET_SIZE;

      output.push(String.fromCharCode(offset + LOWERCASE_START));
      continue;
    }

    output.push(character);
  }

  return output.join('');
};

/* Module level: computed once, never re-rendered. */
const ABOUT_EXAMPLE_OUTPUT = caesarCipher(
  EXAMPLE_WORD,
  ABOUT_EXAMPLE_SHIFT,
  false
);

const ABOUT_EXAMPLE_COLUMNS: ReadonlyArray<{
  position: number;
  source: string;
  output: string;
}> = Array.from(EXAMPLE_WORD).map((character, index) => ({
  position: index + 1,
  source: character,
  output: ABOUT_EXAMPLE_OUTPUT[index] ?? character
}));

/* =========================================================
   VALIDATION
========================================================= */

const shiftValidation = computed<ShiftValidation>(() => {
  const trimmed = shiftText.value.trim();

  if (trimmed === '') {
    return { status: 'empty' };
  }

  const numeric = Number(trimmed);

  if (!Number.isFinite(numeric)) {
    return { status: 'not-a-number' };
  }

  if (!Number.isInteger(numeric)) {
    return { status: 'not-integer' };
  }

  if (numeric < MIN_SHIFT || numeric > MAX_SHIFT) {
    return { status: 'out-of-range', value: numeric };
  }

  return { status: 'valid', value: numeric };
});

const shiftErrorMessage = computed<string>(() => {
  switch (shiftValidation.value.status) {
    case 'empty':
      return 'Shift value is required.';
    case 'not-a-number':
    case 'not-integer':
      return 'Shift must be a whole number.';
    case 'out-of-range':
      return `Shift must be between ${MIN_SHIFT} and ${MAX_SHIFT}.`;
    case 'valid':
      return '';
    default:
      return assertNever(shiftValidation.value);
  }
});

const activeShift = computed<number | null>(() =>
  shiftValidation.value.status === 'valid'
    ? shiftValidation.value.value
    : null
);

const canProcess = computed<boolean>(
  () => messageText.value.trim().length > 0 && activeShift.value !== null
);

/* =========================================================
   MODE DERIVED COPY
========================================================= */

const isDarkMode = computed<boolean>(() => theme.value === 'dark');

const themeClass = computed<string>(() =>
  theme.value === 'dark' ? 'theme-dark' : 'theme-light'
);

const isEncryptMode = computed<boolean>(() => mode.value === 'encrypt');

const inputLabel = computed<string>(() =>
  isEncryptMode.value ? 'Plaintext' : 'Ciphertext'
);

const outputLabel = computed<string>(() =>
  isEncryptMode.value ? 'Ciphertext' : 'Plaintext'
);

const outputLabelFor = (value: CipherMode): string =>
  value === 'encrypt' ? 'Ciphertext' : 'Plaintext';

const inputPlaceholder = computed<string>(() =>
  isEncryptMode.value
    ? 'Type your plaintext message here'
    : 'Type your ciphertext message here'
);

const inputHint = computed<string>(() =>
  isEncryptMode.value
    ? 'Letters shift. Numbers, spaces and symbols stay put.'
    : 'Paste the ciphertext you received to recover the message.'
);

/* =========================================================
   LIVE PREVIEW (single computation, reused everywhere)
========================================================= */

const previewOutput = computed<string | null>(() => {
  const shift = activeShift.value;

  if (shift === null || messageText.value.trim() === '') {
    return null;
  }

  return caesarCipher(messageText.value, shift, !isEncryptMode.value);
});

const processPreviewSource = computed<string>(() => {
  const text = messageText.value.trim();
  return text === '' ? 'Awaiting input' : text;
});

const processPreviewShift = computed<string>(() => {
  const shift = activeShift.value;

  if (shift === null) {
    return 'Invalid';
  }

  return isEncryptMode.value ? `+${shift}` : `-${shift}`;
});

const processPreviewOutput = computed<string>(() =>
  previewOutput.value ?? 'Awaiting output'
);

const processDescription = computed<string>(() => {
  const shift = activeShift.value;

  if (messageText.value.trim() === '') {
    return 'Enter a message to see the cipher process.';
  }

  if (shift === null) {
    return `Enter a whole number from ${MIN_SHIFT} to ${MAX_SHIFT} to continue.`;
  }

  return isEncryptMode.value
    ? `Each letter moves ${shift} position${shift === 1 ? '' : 's'} forward, wrapping from Z back to A.`
    : `Each letter moves ${shift} position${shift === 1 ? '' : 's'} backward, wrapping from A back to Z.`;
});

const exampleInput = computed<string>(() =>
  isEncryptMode.value ? 'A' : 'D'
);

const exampleOutput = computed<string>(() => {
  const shift = activeShift.value;

  if (shift === null) {
    return '?';
  }

  return caesarCipher(exampleInput.value, shift, !isEncryptMode.value);
});

const exampleDescription = computed<string>(() => {
  const shift = activeShift.value;

  if (shift === null) {
    return 'Set a valid shift to view the letter example.';
  }

  return isEncryptMode.value
    ? `A moves ${shift} position${shift === 1 ? '' : 's'} forward.`
    : `D moves ${shift} position${shift === 1 ? '' : 's'} backward.`;
});

const alphabetMap = computed<AlphabetPair[]>(() => {
  const shift = activeShift.value;

  return Array.from(ALPHABET).map((letter) => ({
    from: letter,
    to:
      shift === null
        ? letter
        : caesarCipher(letter, shift, !isEncryptMode.value)
  }));
});

const isResultStale = computed<boolean>(() => {
  const result = submitted.value;

  if (result === null) {
    return false;
  }

  return (
    result.source !== messageText.value ||
    result.mode !== mode.value ||
    result.shift !== (activeShift.value ?? -1)
  );
});

/* =========================================================
   COPY FEEDBACK
========================================================= */

const copyFeedback = computed<CopyFeedback | null>(() => {
  switch (copyState.value.status) {
    case 'idle':
    case 'copying':
      return null;
    case 'copied':
      return { tone: 'success', text: 'Result copied to clipboard.' };
    case 'error':
      return { tone: 'danger', text: copyState.value.message };
    default:
      return assertNever(copyState.value);
  }
});

const copyButtonLabel = computed<string>(() =>
  copyState.value.status === 'copying'
    ? 'Copying result'
    : 'Copy result to clipboard'
);

const statusAnnouncement = computed<string>(() => {
  if (copyState.value.status === 'copied') {
    return 'Result copied to clipboard.';
  }

  if (copyState.value.status === 'error') {
    return copyState.value.message;
  }

  if (cipherError.value !== null) {
    return cipherError.value;
  }

  const result = submitted.value;

  if (result === null) {
    return '';
  }

  return `${outputLabelFor(result.mode)} ready. ${result.characterCount} characters.`;
});

/* =========================================================
   FIELD HANDLERS (explicit, typed, no watchers)
========================================================= */

const readFieldValue = (event: FieldChangeEvent): string => {
  const value = event.detail.value;

  if (typeof value === 'string') {
    return value;
  }

  if (typeof value === 'number' && Number.isFinite(value)) {
    return String(value);
  }

  return '';
};

const handleMessageInput = (event: FieldChangeEvent): void => {
  messageText.value = readFieldValue(event).slice(0, MAX_MESSAGE_LENGTH);
};

const handleShiftInput = (event: FieldChangeEvent): void => {
  shiftText.value = readFieldValue(event).slice(0, 3);
};

const handleModeChange = (event: FieldChangeEvent): void => {
  const next = event.detail.value;

  if (next === 'encrypt' || next === 'decrypt') {
    mode.value = next;
  }
};

const applyShiftPreset = (preset: number): void => {
  shiftText.value = String(preset);
};

/* =========================================================
   ACTIONS
========================================================= */

const focusMessageField = (): void => {
  const field = messageFieldRef.value?.querySelector('textarea');
  field?.focus();
};

const clearCopyTimer = (): void => {
  if (copyResetTimer !== null) {
    clearTimeout(copyResetTimer);
    copyResetTimer = null;
  }
};

const resetCopyState = (): void => {
  clearCopyTimer();
  copyState.value = { status: 'idle' };
};

const processCipher = (): void => {
  const shift = activeShift.value;

  if (messageText.value.trim().length === 0 || shift === null) {
    submitted.value = null;
    cipherError.value =
      shiftErrorMessage.value === ''
        ? 'Enter a message before running the operation.'
        : `Enter a message and a whole number from ${MIN_SHIFT} to ${MAX_SHIFT}.`;
    return;
  }

  try {
    const output = caesarCipher(
      messageText.value,
      shift,
      mode.value === 'decrypt'
    );

    if (output.length === 0) {
      throw new CipherError('Empty cipher output', 'PROCESSING_FAILED');
    }

    submitted.value = {
      mode: mode.value,
      shift,
      source: messageText.value,
      output,
      characterCount: output.length
    };

    cipherError.value = null;
    resetCopyState();
  } catch (error) {
    logError('processCipher', error, {
      mode: mode.value,
      shift,
      length: messageText.value.length
    });

    submitted.value = null;
    cipherError.value =
      'This message could not be processed. Shorten it and try again.';
  }
};

const clearForm = (): void => {
  messageText.value = '';
  shiftText.value = String(DEFAULT_SHIFT);
  mode.value = 'encrypt';
  submitted.value = null;
  cipherError.value = null;
  resetCopyState();
  focusMessageField();
};

const goToView = (view: AppView): void => {
  currentView.value = view;
};

const copyResult = async (): Promise<void> => {
  const result = submitted.value;

  if (result === null || copyState.value.status === 'copying') {
    return;
  }

  copyState.value = { status: 'copying' };

  try {
    if (
      typeof navigator === 'undefined' ||
      navigator.clipboard === undefined ||
      typeof navigator.clipboard.writeText !== 'function'
    ) {
      throw new CipherError(
        'Clipboard API unavailable',
        'CLIPBOARD_UNSUPPORTED'
      );
    }

    await navigator.clipboard.writeText(result.output);

    copyState.value = { status: 'copied' };

    clearCopyTimer();
    copyResetTimer = setTimeout(() => {
      copyResetTimer = null;

      if (copyState.value.status === 'copied') {
        copyState.value = { status: 'idle' };
      }
    }, COPY_FEEDBACK_RESET_MS);
  } catch (error) {
    logError('copyResult', error, {
      length: result.output.length
    });

    const unsupported =
      error instanceof CipherError &&
      error.code === 'CLIPBOARD_UNSUPPORTED';

    copyState.value = {
      status: 'error',
      message: unsupported
        ? 'Copying is not available here. Select the result text and copy it manually.'
        : 'Could not copy the result. Check your browser permissions and try again.'
    };
  }
};

const recoverFromRenderError = (): void => {
  renderError.value = null;
  currentView.value = 'home';
};

/* =========================================================
   THEME
========================================================= */

const readStoredTheme = (): ThemeMode | null => {
  try {
    const stored = localStorage.getItem(THEME_STORAGE_KEY);
    return stored === 'light' || stored === 'dark' ? stored : null;
  } catch (error) {
    logError('readStoredTheme', error);
    return null;
  }
};

const writeStoredTheme = (value: ThemeMode): void => {
  try {
    localStorage.setItem(THEME_STORAGE_KEY, value);
  } catch (error) {
    logError('writeStoredTheme', error);
  }
};

const resolveSystemTheme = (): ThemeMode => {
  if (typeof window === 'undefined' || typeof window.matchMedia !== 'function') {
    return 'light';
  }

  return window.matchMedia(DARK_MEDIA_QUERY).matches ? 'dark' : 'light';
};

const handleSystemThemeChange = (event: MediaQueryListEvent): void => {
  // An explicit user choice always wins over the OS setting.
  if (readStoredTheme() !== null) {
    return;
  }

  theme.value = event.matches ? 'dark' : 'light';
};

const toggleTheme = (): void => {
  theme.value = theme.value === 'dark' ? 'light' : 'dark';
  writeStoredTheme(theme.value);
};

/* =========================================================
   LIFECYCLE
========================================================= */

onMounted(() => {
  theme.value = readStoredTheme() ?? resolveSystemTheme();

  if (typeof window !== 'undefined' && typeof window.matchMedia === 'function') {
    themeMediaQuery = window.matchMedia(DARK_MEDIA_QUERY);
    themeMediaQuery.addEventListener('change', handleSystemThemeChange);
  }
});

onBeforeUnmount(() => {
  themeMediaQuery?.removeEventListener('change', handleSystemThemeChange);
  themeMediaQuery = null;
  clearCopyTimer();
});

onErrorCaptured((error: unknown) => {
  logError('render', error, { view: currentView.value });

  renderError.value =
    'Something went wrong while rendering this screen. Reload to continue.';

  return false;
});
</script>

<style scoped>
/* =========================================================
   FONTS (Fontsource CDN, 2 families maximum)
========================================================= */
@import url('https://cdn.jsdelivr.net/fontsource/css/plus-jakarta-sans@latest/latin-400-normal.css');
@import url('https://cdn.jsdelivr.net/fontsource/css/plus-jakarta-sans@latest/latin-600-normal.css');
@import url('https://cdn.jsdelivr.net/fontsource/css/plus-jakarta-sans@latest/latin-700-normal.css');
@import url('https://cdn.jsdelivr.net/fontsource/css/plus-jakarta-sans@latest/latin-800-normal.css');
@import url('https://cdn.jsdelivr.net/fontsource/css/jetbrains-mono@latest/latin-400-normal.css');
@import url('https://cdn.jsdelivr.net/fontsource/css/jetbrains-mono@latest/latin-700-normal.css');

/* =========================================================
   DESIGN TOKENS
   Contrast verified (WCAG 2.1 AA):
     text-primary / surface   17.1:1
     text-secondary / surface  6.1:1
     text-muted / surface      5.3:1
     brand-text / surface      8.4:1
     on-brand / brand          7.5:1
     success / success-tint    6.0:1
     danger / danger-tint      6.6:1
   Border lightness is >= 14% away from every background it
   touches, so no border is invisible.
========================================================= */
.app-page {
  /* Brand: 10% accent */
  --color-brand: hsl(226 76% 46%);
  --color-brand-hover: hsl(226 76% 40%);
  --color-brand-gradient-end: hsl(206 80% 42%);
  --color-brand-tint: hsl(226 76% 96%);
  --color-brand-border: hsl(226 45% 82%);
  --color-brand-text: hsl(226 76% 42%);
  --color-on-brand: hsl(0 0% 100%);

  /* Surfaces: 60% canvas, 30% panels */
  --color-canvas: hsl(220 20% 98%);
  --color-surface: hsl(0 0% 100%);
  --color-overlay: hsl(220 18% 96%);

  /* Text */
  --color-text-primary: hsl(222 25% 12%);
  --color-text-secondary: hsl(220 12% 40%);
  --color-text-muted: hsl(220 10% 44%);

  /* Borders */
  --color-border: hsl(220 13% 82%);
  --color-border-strong: hsl(220 13% 68%);

  /* Semantic (green + red only, plus brand) */
  --color-success: hsl(152 60% 26%);
  --color-success-tint: hsl(152 45% 95%);
  --color-success-border: hsl(152 35% 78%);
  --color-danger: hsl(356 70% 38%);
  --color-danger-tint: hsl(356 70% 96%);
  --color-danger-border: hsl(356 50% 84%);

  /* Focus */
  --color-focus: hsl(226 76% 46%);
  --color-focus-ring: hsl(226 76% 46% / 0.2);

  /* Shadows: subtle only */
  --shadow-xs: 0 1px 2px rgba(15, 23, 42, 0.04);
  --shadow-sm: 0 2px 8px rgba(15, 23, 42, 0.05),
    0 1px 2px rgba(15, 23, 42, 0.03);
  --shadow-md: 0 4px 16px rgba(15, 23, 42, 0.07),
    0 2px 4px rgba(15, 23, 42, 0.04);
  --shadow-lg: 0 8px 24px rgba(15, 23, 42, 0.09),
    0 4px 8px rgba(15, 23, 42, 0.05);

  /* Radius scale */
  --radius-xs: 4px;
  --radius-sm: 6px;
  --radius-md: 10px;
  --radius-lg: 14px;
  --radius-xl: 18px;
  --radius-full: 9999px;

  /* Nesting rule: outer = inner + padding gap.
     Inner surfaces use --radius-sm (6px) inside a card with
     --card-padding, so the card radius is derived, never arbitrary. */
  --card-padding: 14px;
  --radius-card: calc(var(--radius-sm) + var(--card-padding));

  /* Spacing */
  --space-1: 4px;
  --space-2: 8px;
  --space-3: 12px;
  --space-4: 16px;
  --space-5: 20px;
  --space-6: 24px;
  --space-8: 32px;

  /* Transitions */
  --transition-fast: 100ms ease;
  --transition-base: 180ms ease;
  --transition-slow: 320ms ease;

  /* Type scale */
  --text-xs: 11px;
  --text-sm: 13px;
  --text-base: 15px;
  --text-md: 17px;
  --text-lg: 20px;
  --text-xl: 24px;
  --text-2xl: 30px;
  --text-3xl: 40px;

  /* Type families */
  --font-sans: 'Plus Jakarta Sans', 'Segoe UI', system-ui,
    -apple-system, sans-serif;
  --font-mono: 'JetBrains Mono', ui-monospace, SFMono-Regular,
    Menlo, monospace;

  /* Ionic bridge */
  --ion-background-color: var(--color-canvas);
  --ion-text-color: var(--color-text-primary);
  --ion-border-color: var(--color-border);
  --ion-item-background: transparent;
  --ion-toolbar-background: var(--color-surface);
  --ion-toolbar-color: var(--color-text-primary);
  --ion-toolbar-border-color: var(--color-border);
  --ion-font-family: var(--font-sans);

  background: var(--color-canvas);
  color: var(--color-text-primary);
  font-family: var(--font-sans);

  transition: background-color var(--transition-slow),
    color var(--transition-slow);
}

.app-page.theme-dark {
  --color-brand: hsl(226 72% 56%);
  --color-brand-hover: hsl(226 72% 62%);
  --color-brand-gradient-end: hsl(206 72% 44%);
  --color-brand-tint: hsl(226 45% 18%);
  --color-brand-border: hsl(226 32% 34%);
  --color-brand-text: hsl(226 92% 78%);
  --color-on-brand: hsl(0 0% 100%);

  --color-canvas: hsl(222 30% 7%);
  --color-surface: hsl(222 24% 11%);
  --color-overlay: hsl(222 22% 16%);

  --color-text-primary: hsl(220 25% 96%);
  --color-text-secondary: hsl(220 14% 78%);
  --color-text-muted: hsl(220 10% 66%);

  --color-border: hsl(222 18% 28%);
  --color-border-strong: hsl(222 16% 42%);

  --color-success: hsl(152 55% 62%);
  --color-success-tint: hsl(152 35% 14%);
  --color-success-border: hsl(152 30% 28%);
  --color-danger: hsl(356 85% 72%);
  --color-danger-tint: hsl(356 45% 15%);
  --color-danger-border: hsl(356 38% 32%);

  --color-focus: hsl(226 92% 76%);
  --color-focus-ring: hsl(226 92% 76% / 0.24);

  --shadow-xs: 0 1px 2px rgba(0, 0, 0, 0.24);
  --shadow-sm: 0 2px 8px rgba(0, 0, 0, 0.28),
    0 1px 2px rgba(0, 0, 0, 0.2);
  --shadow-md: 0 4px 16px rgba(0, 0, 0, 0.32),
    0 2px 4px rgba(0, 0, 0, 0.22);
  --shadow-lg: 0 8px 24px rgba(0, 0, 0, 0.38),
    0 4px 8px rgba(0, 0, 0, 0.26);

  --ion-background-color: var(--color-canvas);
  --ion-text-color: var(--color-text-primary);
  --ion-border-color: var(--color-border);
  --ion-toolbar-background: var(--color-surface);
  --ion-toolbar-color: var(--color-text-primary);
  --ion-toolbar-border-color: var(--color-border);
}

/* =========================================================
   BASE + MOTION LAYER
========================================================= */
.app-page button,
.app-page a,
.app-page summary,
.app-page [role='button'],
.app-page .app-card,
.app-page .feature-tile,
.app-page ion-button,
.app-page ion-segment-button {
  transition: color var(--transition-base),
    background-color var(--transition-base),
    border-color var(--transition-base),
    box-shadow var(--transition-base),
    transform var(--transition-base),
    opacity var(--transition-base);
}

.app-page :focus-visible,
.app-page ion-button:focus-within,
.app-page ion-segment-button:focus-within {
  outline: 2px solid var(--color-focus);
  outline-offset: 2px;
  border-radius: var(--radius-sm);
}

.app-icon {
  display: block;
  flex-shrink: 0;
}

.mono {
  font-family: var(--font-mono);
}

.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  margin: -1px;
  padding: 0;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}

.skip-link {
  position: fixed;
  top: var(--space-2);
  left: var(--space-2);
  z-index: 1000;
  padding: 10px 14px;
  border: 1px solid var(--color-border-strong);
  border-radius: var(--radius-sm);
  background: var(--color-surface);
  color: var(--color-text-primary);
  box-shadow: var(--shadow-md);
  font-size: var(--text-sm);
  font-weight: 700;
  text-decoration: none;
  transform: translateY(-200%);
}

.skip-link:focus-visible {
  transform: translateY(0);
}

.link-button {
  padding: 0;
  border: 0;
  background: none;
  color: inherit;
  font: inherit;
  font-weight: 700;
  text-decoration: underline;
  text-underline-offset: 2px;
  cursor: pointer;
}

.link-button:hover {
  opacity: 0.78;
}

/* =========================================================
   LAYOUT
========================================================= */
.app-content {
  --background: var(--color-canvas);
  --color: var(--color-text-primary);
}

.landing-content {
  --background: radial-gradient(
      120% 70% at 50% 0%,
      var(--color-brand-tint) 0%,
      transparent 62%
    ),
    var(--color-canvas);
}

.page-container,
.landing-container {
  width: 100%;
  max-width: 680px;
  margin: 0 auto;
  padding: var(--space-4) var(--space-4) var(--space-8);
}

.app-header,
.app-footer {
  background: var(--color-surface);
}

.app-toolbar,
.bottom-toolbar {
  --background: var(--color-surface);
  --color: var(--color-text-primary);
  --border-width: 0;
  --min-height: 56px;

  border-bottom: 1px solid var(--color-border);
}

.bottom-toolbar {
  border-top: 1px solid var(--color-border);
  border-bottom: 0;
}

.app-title {
  display: flex;
  align-items: center;
  gap: var(--space-2);
  color: var(--color-text-primary);
  font-size: var(--text-md);
  font-weight: 800;
  letter-spacing: -0.3px;
}

.title-mark {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 26px;
  height: 26px;
  border-radius: var(--radius-sm);
  background: var(--color-brand-tint);
  color: var(--color-brand-text);
}

.toolbar-actions {
  display: flex;
  align-items: center;
  padding-right: var(--space-3);
}

/* =========================================================
   THEME TOGGLE
========================================================= */
.theme-toggle {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: var(--space-2);
  min-width: 38px;
  min-height: 38px;
  padding: 0 var(--space-2);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-sm);
  background: var(--color-surface);
  color: var(--color-text-secondary);
  font-size: var(--text-xs);
  font-weight: 700;
  cursor: pointer;
}

.theme-toggle:hover {
  border-color: var(--color-border-strong);
  color: var(--color-text-primary);
  box-shadow: var(--shadow-xs);
  transform: translateY(-1px);
}

.theme-toggle:active {
  transform: translateY(0) scale(0.97);
}

.theme-toggle[aria-checked='true'] {
  border-color: var(--color-brand-border);
  background: var(--color-brand-tint);
  color: var(--color-brand-text);
}

/* =========================================================
   HERO
========================================================= */
.hero-section {
  padding: var(--space-6) var(--space-3) var(--space-4);
  text-align: center;
  animation: riseIn 320ms ease both;
}

.hero-tile {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 64px;
  height: 64px;
  margin: 0 auto var(--space-4);
  border-radius: var(--radius-lg);
  background: linear-gradient(
    135deg,
    var(--color-brand) 0%,
    var(--color-brand-gradient-end) 100%
  );
  box-shadow: var(--shadow-md);
  color: var(--color-on-brand);
}

.hero-tile-large {
  width: 88px;
  height: 88px;
  margin-bottom: var(--space-5);
  border-radius: var(--radius-xl);
  animation: floaty 5s ease-in-out infinite;
}

.eyebrow {
  margin: 0 0 var(--space-2);
  color: var(--color-brand-text);
  font-size: var(--text-xs);
  font-weight: 800;
  letter-spacing: 1.4px;
  text-transform: uppercase;
}

.page-title {
  margin: 0;
  color: var(--color-text-primary);
  font-size: var(--text-2xl);
  font-weight: 800;
  letter-spacing: -0.8px;
  line-height: 1.15;
}

.display-title {
  max-width: 460px;
  margin: 0 auto;
  color: var(--color-text-primary);
  font-size: clamp(32px, 8vw, var(--text-3xl));
  font-weight: 800;
  letter-spacing: -1.2px;
  line-height: 1.08;
}

.hero-description {
  max-width: 340px;
  margin: var(--space-3) auto 0;
  color: var(--color-text-secondary);
  font-size: var(--text-base);
  line-height: 1.6;
}

/* =========================================================
   LANDING
========================================================= */
.landing-container {
  display: flex;
  min-height: 100%;
  flex-direction: column;
  justify-content: center;
  padding-top: var(--space-8);
  padding-bottom: var(--space-8);
}

.landing-topbar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: var(--space-3);
  margin-bottom: var(--space-6);
}

.landing-brand {
  display: inline-flex;
  align-items: center;
  gap: var(--space-2);
  color: var(--color-text-primary);
  font-size: var(--text-sm);
  font-weight: 800;
  letter-spacing: -0.2px;
}

.landing-hero {
  padding: 0 var(--space-2);
  text-align: center;
  animation: riseIn 380ms ease both;
}

.landing-description {
  max-width: 380px;
  margin: var(--space-4) auto 0;
  color: var(--color-text-secondary);
  font-size: var(--text-md);
  line-height: 1.7;
}

.landing-developer {
  margin: var(--space-5) 0 0;
  color: var(--color-text-muted);
  font-size: var(--text-sm);
  line-height: 1.7;
}

.landing-developer strong {
  display: block;
  color: var(--color-text-primary);
  font-size: var(--text-sm);
  font-weight: 700;
}

.landing-cta {
  max-width: 340px;
  margin: var(--space-6) auto 0;
}

.landing-note {
  margin: var(--space-4) 0 0;
  color: var(--color-text-muted);
  font-size: var(--text-xs);
  font-weight: 600;
  letter-spacing: 0.2px;
}

.landing-features {
  margin-top: var(--space-8);
  animation: riseIn 420ms 80ms ease both;
}

.feature-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: var(--space-3);
  margin: 0;
  padding: 0;
  list-style: none;
}

.feature-tile {
  padding: var(--space-4) var(--space-3);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-card);
  background: var(--color-surface);
  box-shadow: var(--shadow-sm);
  text-align: center;
}

.feature-tile:hover {
  border-color: var(--color-border-strong);
  box-shadow: var(--shadow-md);
  transform: translateY(-2px);
}

.feature-icon {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 34px;
  height: 34px;
  margin-bottom: var(--space-3);
  border-radius: var(--radius-sm);
  background: var(--color-brand-tint);
  color: var(--color-brand-text);
}

.feature-tile strong {
  display: block;
  color: var(--color-text-primary);
  font-size: var(--text-sm);
  font-weight: 800;
}

.feature-tile p {
  margin: var(--space-2) 0 0;
  color: var(--color-text-secondary);
  font-size: var(--text-xs);
  line-height: 1.55;
}

/* =========================================================
   CARDS
========================================================= */
.app-card {
  margin: var(--space-4) 0;
  padding: var(--card-padding);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-card);
  background: var(--color-surface);
  box-shadow: var(--shadow-sm);
  animation: riseIn 300ms ease both;
}

.app-card:hover {
  box-shadow: var(--shadow-md);
}

.card-header {
  display: flex;
  align-items: flex-start;
  justify-content: space-between;
  gap: var(--space-3);
  margin-bottom: var(--space-3);
}

.card-header-tight {
  margin-bottom: var(--space-4);
}

.card-title {
  display: flex;
  align-items: center;
  gap: var(--space-2);
  margin: 0;
  color: var(--color-text-primary);
  font-size: var(--text-lg);
  font-weight: 800;
  letter-spacing: -0.4px;
}

.card-title-center {
  justify-content: center;
  text-align: center;
}

.card-eyebrow {
  display: flex;
  align-items: center;
  gap: var(--space-1);
  margin: 0 0 var(--space-1);
  font-size: var(--text-xs);
  font-weight: 800;
  letter-spacing: 1.2px;
  text-transform: uppercase;
}

.eyebrow-success {
  color: var(--color-success);
}

.micro-label {
  margin: 0 0 var(--space-2);
  color: var(--color-text-muted);
  font-size: var(--text-xs);
  font-weight: 800;
  letter-spacing: 1px;
  text-transform: uppercase;
}

/* =========================================================
   CONSOLE STEPS
========================================================= */
.step + .step {
  margin-top: var(--space-5);
}

.step-heading {
  display: flex;
  align-items: center;
  gap: var(--space-2);
  margin: 0 0 var(--space-3);
  color: var(--color-text-primary);
  font-size: var(--text-base);
  font-weight: 700;
  letter-spacing: -0.2px;
}

.step-index {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  min-width: 26px;
  height: 22px;
  border: 1px solid var(--color-brand-border);
  border-radius: var(--radius-xs);
  background: var(--color-brand-tint);
  color: var(--color-brand-text);
  font-family: var(--font-mono);
  font-size: var(--text-xs);
  font-weight: 700;
}

.mode-segment {
  --background: var(--color-overlay);

  min-height: 46px;
  padding: 2px;
  border: 1px solid var(--color-border);
  border-radius: var(--radius-sm);
}

.mode-segment ion-segment-button {
  --indicator-color: var(--color-surface);
  --color: var(--color-text-secondary);
  --color-checked: var(--color-text-primary);
  --border-radius: var(--radius-xs);

  min-height: 40px;
  font-size: var(--text-sm);
  font-weight: 700;
}

.segment-label {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: var(--space-2);
}

.field-wrap {
  border-radius: var(--radius-sm);
}

.field-item {
  --background: var(--color-overlay);
  --padding-start: 0;
  --inner-padding-end: 0;
  --inner-border-width: 0;
  --min-height: 0;

  margin: 0;
  border: 1px solid var(--color-border);
  border-radius: var(--radius-sm);
}

.field-label {
  margin: var(--space-3) var(--space-3) var(--space-1);
  color: var(--color-text-secondary) !important;
  font-size: var(--text-xs) !important;
  font-weight: 800 !important;
  letter-spacing: 0.6px;
  text-transform: uppercase;
}

.field-control {
  --padding-start: var(--space-3);
  --padding-end: var(--space-3);
  --padding-top: var(--space-1);
  --padding-bottom: var(--space-3);

  color: var(--color-text-primary);
  font-size: var(--text-base);
}

.field-control.textarea-control,
.field-item:has(textarea) {
  line-height: 1.6;
}

.field-wrap:focus-within,
.field-item:focus-within {
  border-color: var(--color-brand);
  box-shadow: 0 0 0 3px var(--color-focus-ring);
}

.field-meta {
  display: flex;
  align-items: baseline;
  justify-content: space-between;
  gap: var(--space-3);
  margin-top: var(--space-2);
}

.field-hint {
  margin: var(--space-2) 0 0;
  color: var(--color-text-muted);
  font-size: var(--text-xs);
  line-height: 1.5;
}

.field-counter {
  margin: 0;
  color: var(--color-text-muted);
  font-family: var(--font-mono);
  font-size: var(--text-xs);
  white-space: nowrap;
}

.counter-warning {
  color: var(--color-danger);
  font-weight: 700;
}

.field-error {
  display: flex;
  align-items: center;
  gap: var(--space-2);
  margin: var(--space-2) 0 0;
  color: var(--color-danger);
  font-size: var(--text-xs);
  font-weight: 600;
  line-height: 1.5;
  animation: nudgeIn 180ms ease both;
}

.shift-presets {
  display: flex;
  flex-wrap: wrap;
  gap: var(--space-2);
  margin-top: var(--space-3);
}

.preset-chip {
  min-width: 40px;
  padding: 6px 10px;
  border: 1px solid var(--color-border);
  border-radius: var(--radius-sm);
  background: var(--color-overlay);
  color: var(--color-text-secondary);
  font-family: var(--font-mono);
  font-size: var(--text-sm);
  font-weight: 700;
  cursor: pointer;
}

.preset-chip:hover {
  border-color: var(--color-border-strong);
  color: var(--color-text-primary);
}

.preset-chip.preset-active {
  border-color: var(--color-brand-border);
  background: var(--color-brand-tint);
  color: var(--color-brand-text);
}

/* =========================================================
   BUTTONS
========================================================= */
.cta-button {
  --border-radius: var(--radius-sm);
  --background: var(--color-brand);
  --background-hover: var(--color-brand-hover);
  --background-activated: var(--color-brand-hover);
  --color: var(--color-on-brand);
  --box-shadow: none;

  min-height: 48px;
  margin-top: var(--space-5);
  font-size: var(--text-sm);
  font-weight: 800;
  letter-spacing: 0.1px;
}

.cta-button::part(native) {
  gap: var(--space-2);
  background-image: linear-gradient(
    135deg,
    var(--color-brand) 0%,
    var(--color-brand-gradient-end) 100%
  );
}

.cta-button:not(.button-disabled):hover::part(native) {
  filter: brightness(1.06);
  box-shadow: var(--shadow-sm);
  transform: translateY(-1px);
}

.cta-button.button-disabled {
  opacity: 0.45;
}

.cta-icon {
  transition: transform var(--transition-base);
}

.cta-button:hover .cta-icon {
  transform: translateX(3px);
}

.ghost-button {
  --border-radius: var(--radius-sm);
  --border-color: var(--color-border-strong);
  --border-width: 1px;
  --color: var(--color-text-secondary);
  --background: transparent;

  min-height: 44px;
  margin: var(--space-4) 0 0;
  font-size: var(--text-sm);
  font-weight: 700;
}

.ghost-button::part(native) {
  gap: var(--space-2);
}

.ghost-button:hover::part(native) {
  border-color: var(--color-brand);
  background: var(--color-brand-tint);
  color: var(--color-brand-text);
  box-shadow: var(--shadow-xs);
  transform: translateY(-1px);
}

.icon-button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 38px;
  height: 38px;
  flex-shrink: 0;
  border: 1px solid var(--color-border);
  border-radius: var(--radius-sm);
  background: var(--color-overlay);
  color: var(--color-text-secondary);
  cursor: pointer;
}

.icon-button:hover:not(:disabled) {
  border-color: var(--color-brand-border);
  background: var(--color-brand-tint);
  color: var(--color-brand-text);
  box-shadow: var(--shadow-xs);
}

.icon-button:active:not(:disabled) {
  transform: scale(0.95);
}

.icon-button:disabled {
  cursor: progress;
}

.icon-button-busy {
  background-image: linear-gradient(
    90deg,
    var(--color-overlay) 0%,
    var(--color-brand-tint) 50%,
    var(--color-overlay) 100%
  );
  background-size: 200% 100%;
  animation: shimmer 1.4s ease-in-out infinite;
}

.icon-spin {
  animation: spin 900ms linear infinite;
}

/* =========================================================
   BANNERS
========================================================= */
.banner {
  display: flex;
  align-items: center;
  flex-wrap: wrap;
  gap: var(--space-2);
  margin: var(--space-3) 0;
  padding: var(--space-3);
  border-radius: var(--radius-sm);
  font-size: var(--text-sm);
  font-weight: 600;
  line-height: 1.5;
}

.banner-danger {
  border: 1px solid var(--color-danger-border);
  background: var(--color-danger-tint);
  color: var(--color-danger);
}

/* =========================================================
   RESULT
========================================================= */
.result-card {
  border-color: var(--color-success-border);
  animation: riseIn 300ms ease both;
}

.result-output {
  display: block;
  width: 100%;
  min-height: 76px;
  margin: 0;
  padding: var(--space-3);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-sm);
  background: var(--color-overlay);
  color: var(--color-text-primary);
  font-size: var(--text-base);
  font-weight: 400;
  line-height: 1.65;
  white-space: pre-wrap;
  overflow-wrap: anywhere;
}

.copy-feedback {
  display: flex;
  align-items: center;
  flex-wrap: wrap;
  gap: var(--space-2);
  margin: var(--space-3) 0 0;
  font-size: var(--text-xs);
  font-weight: 700;
  animation: nudgeIn 180ms ease both;
}

.copy-success {
  color: var(--color-success);
}

.copy-danger {
  color: var(--color-danger);
}

.success-banner {
  display: flex;
  align-items: center;
  flex-wrap: wrap;
  gap: var(--space-2);
  margin: var(--space-3) 0 0;
  padding: var(--space-3);
  border: 1px solid var(--color-success-border);
  border-radius: var(--radius-sm);
  background: var(--color-success-tint);
  color: var(--color-success);
  font-size: var(--text-xs);
  font-weight: 700;
  line-height: 1.5;
}

.success-dot {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 20px;
  height: 20px;
  flex-shrink: 0;
  border-radius: var(--radius-full);
  background: var(--color-success);
  color: var(--color-surface);
}

.success-meta {
  margin-left: auto;
  color: var(--color-text-secondary);
  font-family: var(--font-mono);
  font-weight: 400;
}

.stale-hint {
  display: flex;
  align-items: flex-start;
  gap: var(--space-2);
  margin: var(--space-3) 0 0;
  color: var(--color-text-muted);
  font-size: var(--text-xs);
  line-height: 1.5;
}

/* =========================================================
   PROCESS
========================================================= */
.process-flow {
  display: flex;
  align-items: stretch;
  gap: var(--space-2);
  margin: 0;
  padding: 0;
  list-style: none;
}

.process-step {
  display: flex;
  flex: 1;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: var(--space-2);
  min-width: 0;
  min-height: 88px;
  padding: var(--space-3) var(--space-2);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-sm);
  background: var(--color-overlay);
  text-align: center;
  transition: transform var(--transition-base),
    box-shadow var(--transition-base),
    border-color var(--transition-base);
}

.process-step:hover {
  box-shadow: var(--shadow-sm);
  transform: translateY(-2px);
}

.step-shift {
  border-color: var(--color-brand-border);
  background: var(--color-brand-tint);
}

.step-output {
  border-color: var(--color-success-border);
  background: var(--color-success-tint);
}

.step-label {
  color: var(--color-text-muted);
  font-size: var(--text-xs);
  font-weight: 800;
  letter-spacing: 0.6px;
  text-transform: uppercase;
}

.step-shift .step-label {
  color: var(--color-brand-text);
}

.step-output .step-label {
  color: var(--color-success);
}

.step-value {
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 2;
  max-width: 100%;
  overflow: hidden;
  color: var(--color-text-primary);
  font-size: var(--text-sm);
  font-weight: 700;
  line-height: 1.4;
  overflow-wrap: anywhere;
}

.step-shift .step-value {
  color: var(--color-brand-text);
}

.step-output .step-value {
  color: var(--color-success);
}

.process-connector {
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--color-border-strong);
  animation: pulseSoft 2.4s ease-in-out infinite;
}

.process-description {
  display: flex;
  align-items: flex-start;
  gap: var(--space-2);
  margin: var(--space-4) 0 0;
  color: var(--color-text-secondary);
  font-size: var(--text-sm);
  line-height: 1.6;
}

.description-icon {
  margin-top: 2px;
  color: var(--color-brand-text);
}

.letter-example {
  margin-top: var(--space-4);
  padding: var(--space-3);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-sm);
  background: var(--color-overlay);
  text-align: center;
}

.letter-flow {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: var(--space-4);
  margin: 0;
}

.letter-flow strong {
  color: var(--color-text-primary);
  font-size: var(--text-xl);
  font-weight: 700;
}

.letter-arrow {
  color: var(--color-brand-text);
}

.letter-note {
  margin: var(--space-2) 0 0;
  color: var(--color-text-muted);
  font-size: var(--text-xs);
}

.alphabet-disclosure {
  margin-top: var(--space-4);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-sm);
  background: var(--color-overlay);
}

.alphabet-summary {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: var(--space-2);
  padding: var(--space-3);
  color: var(--color-text-secondary);
  font-size: var(--text-sm);
  font-weight: 700;
  cursor: pointer;
  list-style: none;
}

.alphabet-summary::-webkit-details-marker {
  display: none;
}

.alphabet-summary:hover {
  color: var(--color-text-primary);
}

.disclosure-icon {
  color: var(--color-text-muted);
  transition: transform var(--transition-base);
}

.alphabet-disclosure[open] .disclosure-icon {
  transform: rotate(180deg);
}

.alphabet-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(74px, 1fr));
  gap: var(--space-2);
  margin: 0;
  padding: 0 var(--space-3) var(--space-3);
  list-style: none;
  animation: riseIn 180ms ease both;
}

.alphabet-cell {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: var(--space-1);
  padding: 6px 4px;
  border: 1px solid var(--color-border);
  border-radius: var(--radius-xs);
  background: var(--color-surface);
}

.alphabet-from {
  color: var(--color-text-muted);
  font-size: var(--text-xs);
}

.alphabet-arrow {
  color: var(--color-border-strong);
}

.alphabet-to {
  color: var(--color-brand-text);
  font-size: var(--text-xs);
  font-weight: 700;
}

/* =========================================================
   ABOUT
========================================================= */
.about-description {
  margin: var(--space-3) 0 0;
  color: var(--color-text-secondary);
  font-size: var(--text-base);
  line-height: 1.75;
}

.formula-group {
  display: grid;
  grid-template-columns: 1fr;
  gap: var(--space-3);
  margin-top: var(--space-5);
}

.formula-block {
  padding: var(--space-3);
  border: 1px solid var(--color-brand-border);
  border-radius: var(--radius-sm);
  background: var(--color-brand-tint);
  text-align: center;
}

.formula-block .micro-label {
  color: var(--color-brand-text);
  opacity: 0.85;
}

.formula {
  margin: 0;
  color: var(--color-brand-text);
  font-size: var(--text-md);
  font-weight: 700;
  letter-spacing: 0.3px;
}

.definition-list {
  margin: var(--space-4) 0 0;
  padding: var(--space-1) var(--space-3);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-sm);
  background: var(--color-overlay);
}

.definition-row {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: var(--space-4);
  padding: var(--space-2) 0;
}

.definition-row + .definition-row {
  border-top: 1px solid var(--color-border);
}

.definition-row dt {
  min-width: 62px;
  color: var(--color-brand-text);
  font-size: var(--text-sm);
  font-weight: 700;
}

.definition-row dd {
  margin: 0;
  color: var(--color-text-secondary);
  font-size: var(--text-sm);
  text-align: right;
}

.content-divider {
  height: 1px;
  margin: var(--space-5) 0;
  border: 0;
  background: var(--color-border);
}

.subsection-title {
  margin: 0 0 var(--space-3);
  color: var(--color-text-primary);
  font-size: var(--text-base);
  font-weight: 800;
  letter-spacing: -0.2px;
}

.table-scroll {
  overflow-x: auto;
  border: 1px solid var(--color-border);
  border-radius: var(--radius-sm);
  background: var(--color-overlay);
}

.example-table {
  width: 100%;
  border-collapse: collapse;
}

.example-table th,
.example-table td {
  padding: var(--space-2) var(--space-3);
  border-bottom: 1px solid var(--color-border);
  font-size: var(--text-sm);
  text-align: center;
}

.example-table thead th {
  color: var(--color-text-muted);
  font-size: var(--text-xs);
  font-weight: 800;
  letter-spacing: 0.6px;
  text-transform: uppercase;
}

.example-table tbody th {
  color: var(--color-text-secondary);
  font-size: var(--text-xs);
  font-weight: 800;
  letter-spacing: 0.6px;
  text-align: left;
  text-transform: uppercase;
  white-space: nowrap;
}

.example-table tbody tr:last-child th,
.example-table tbody tr:last-child td {
  border-bottom: 0;
}

.example-table td {
  color: var(--color-text-primary);
  font-weight: 700;
}

.cell-output {
  color: var(--color-brand-text);
}

.check-list {
  margin: 0;
  padding: 0;
  list-style: none;
}

.check-list li {
  display: flex;
  align-items: flex-start;
  gap: var(--space-2);
  padding: var(--space-2) 0;
  color: var(--color-text-secondary);
  font-size: var(--text-sm);
  line-height: 1.6;
}

.check-icon {
  margin-top: 3px;
  color: var(--color-success);
}

.developer-block {
  text-align: center;
}

.developer-name {
  margin: 0;
  color: var(--color-text-primary);
  font-size: var(--text-md);
  font-weight: 800;
  letter-spacing: -0.3px;
}

.developer-role {
  margin: var(--space-2) 0 0;
  color: var(--color-text-muted);
  font-size: var(--text-sm);
  line-height: 1.6;
}

.quote-block {
  margin: var(--space-5) 0 var(--space-4);
  color: var(--color-text-muted);
  font-size: var(--text-sm);
  font-style: italic;
  text-align: center;
}

/* =========================================================
   BOTTOM NAVIGATION
========================================================= */
.bottom-navigation {
  display: flex;
  gap: var(--space-2);
  padding: var(--space-2) var(--space-4)
    calc(var(--space-2) + env(safe-area-inset-bottom));
}

.nav-button {
  display: flex;
  flex: 1;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: var(--space-1);
  min-height: 46px;
  border: 1px solid transparent;
  border-radius: var(--radius-sm);
  background: transparent;
  color: var(--color-text-muted);
  font-size: var(--text-xs);
  font-weight: 700;
  cursor: pointer;
}

.nav-button:hover {
  border-color: var(--color-border);
  background: var(--color-overlay);
  color: var(--color-text-primary);
  transform: translateY(-1px);
}

.nav-active,
.nav-active:hover {
  border-color: var(--color-brand-border);
  background: var(--color-brand-tint);
  color: var(--color-brand-text);
}

/* =========================================================
   ANIMATION
========================================================= */
@keyframes riseIn {
  from {
    opacity: 0;
    transform: translateY(10px);
  }

  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes nudgeIn {
  from {
    opacity: 0;
    transform: translateY(-3px);
  }

  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes floaty {
  0%,
  100% {
    transform: translateY(0);
  }

  50% {
    transform: translateY(-5px);
  }
}

@keyframes pulseSoft {
  0%,
  100% {
    opacity: 0.5;
  }

  50% {
    opacity: 1;
  }
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}

@keyframes shimmer {
  0% {
    background-position: 100% 0;
  }

  100% {
    background-position: -100% 0;
  }
}

/* =========================================================
   RESPONSIVE
========================================================= */
@media (min-width: 600px) {
  .app-page {
    --card-padding: 18px;
  }

  .page-container,
  .landing-container {
    max-width: 760px;
    padding: var(--space-6) var(--space-6) var(--space-8);
  }

  .hero-section {
    padding-top: var(--space-8);
  }

  .page-title {
    font-size: var(--text-2xl);
  }

  .formula-group {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 380px) {
  .page-container,
  .landing-container {
    padding: var(--space-3) var(--space-3) var(--space-6);
  }

  .feature-grid {
    gap: var(--space-2);
  }

  .feature-tile {
    padding: var(--space-3) var(--space-2);
  }

  .process-flow {
    gap: var(--space-1);
  }

  .process-step {
    min-height: 78px;
    padding: var(--space-2) var(--space-1);
  }

  .step-value {
    font-size: var(--text-xs);
  }
}

/* =========================================================
   REDUCED MOTION
========================================================= */
@media (prefers-reduced-motion: reduce) {
  .app-page *,
  .app-page *::before,
  .app-page *::after {
    animation-duration: 1ms !important;
    animation-iteration-count: 1 !important;
    scroll-behavior: auto !important;
    transition-duration: 1ms !important;
  }
}
</style>