<script lang="ts">
  // Langue et thème, à droite de l'en-tête du tableau de bord. Mêmes stores
  // et même ContextMenu que la barre de titre (où ces deux menus sont
  // aujourd'hui commentés), habillés comme le reste du tableau de bord.
  // Le bouton de thème porte .theme-switcher : c'est la cible de l'étape
  // « Theme » de la visite d'accueil (tourManager.js).
  import { _, locale as svelteLocale } from 'svelte-i18n'
  import ContextMenu from '../../../common/contextMenu/ContextMenu.svelte'
  import { setLocale } from '../../../../libs/i18n'
  import { getFlagEmoji } from '../../../../extensions/tiptap/CustomTranslationExtension/utils/utils'
  import {
    _$_resolvedTheme,
    _$_themeMode,
    themeOptions,
    type ThemeMode
  } from '../../../../stores/theme.store'

  const languageOptions = [
    { code: 'en', labelKey: 'English', native: 'English' },
    { code: 'fr', labelKey: 'French', native: 'Français' },
    { code: 'ja', labelKey: 'Japanese', native: '日本語' }
  ]

  const themeLabelKeys: Record<ThemeMode, string> = {
    system: 'Automatic',
    light: 'Light',
    dark: 'Dark'
  }

  let currentLocale = $derived(String($svelteLocale || 'en').split(/[-_]/)[0])
  let themeMode = $derived($_$_themeMode)
  let resolvedTheme = $derived($_$_resolvedTheme)
  let themeIcon = $derived(
    themeMode === 'system'
      ? 'ph-desktop'
      : resolvedTheme === 'dark'
        ? 'ph-moon-stars'
        : 'ph-sun-dim'
  )
</script>

<div class="flex items-center gap-1.5 shrink-0">
  <ContextMenu __placement="bottom-end" __offset={[0, 8]}>
    <button class="pref-btn" title={$_('Switch language')} aria-label={$_('Switch language')}>
      <i class="ph-light ph-translate text-[16px]" aria-hidden="true"></i>
      <span class="pref-btn__code">{currentLocale.toUpperCase()}</span>
    </button>
    <svelte:fragment slot="content">
      <div class="pref-menu" role="menu">
        <div class="pref-menu__title">{$_('Language')}</div>
        {#each languageOptions as lang}
          <button
            class="pref-item"
            class:pref-item--on={currentLocale === lang.code}
            role="menuitemradio"
            aria-checked={currentLocale === lang.code}
            onclick={() => setLocale(lang.code)}
          >
            <span aria-hidden="true">{getFlagEmoji(lang.code)}</span>
            <span class="flex-1 min-w-0 truncate">{$_(lang.labelKey)}</span>
            <span class="pref-item__meta">{lang.native}</span>
            <i
              class="ph-bold ph-check text-[12px] pref-item__check"
              class:invisible={currentLocale !== lang.code}
              aria-hidden="true"
            ></i>
          </button>
        {/each}
      </div>
    </svelte:fragment>
  </ContextMenu>

  <ContextMenu __placement="bottom-end" __offset={[0, 8]}>
    <button
      class="pref-btn pref-btn--icon theme-switcher"
      title={$_('Switch theme')}
      aria-label={$_('Switch theme')}
    >
      <i class="ph-light {themeIcon} text-[16px]" aria-hidden="true"></i>
    </button>
    <svelte:fragment slot="content">
      <div class="pref-menu" role="menu">
        <div class="pref-menu__title">{$_('Appearance')}</div>
        {#each themeOptions as option}
          <button
            class="pref-item"
            class:pref-item--on={themeMode === option.mode}
            role="menuitemradio"
            aria-checked={themeMode === option.mode}
            onclick={() => _$_themeMode.set(option.mode)}
          >
            <i class="ph-light {option.icon} text-[15px]" aria-hidden="true"></i>
            <span class="flex-1 min-w-0 truncate">{$_(themeLabelKeys[option.mode])}</span>
            <i
              class="ph-bold ph-check text-[12px] pref-item__check"
              class:invisible={themeMode !== option.mode}
              aria-hidden="true"
            ></i>
          </button>
        {/each}
      </div>
    </svelte:fragment>
  </ContextMenu>
</div>

<style>
  /* Même matière que .dash-nav et .dash-search dans l'en-tête : fond
     base-200, liseré base-300, encre à 70 %. */
  .pref-btn {
    display: inline-flex;
    align-items: center;
    gap: 6px;
    height: 40px;
    padding: 0 12px;
    border-radius: 10px;
    border: 1px solid var(--color-base-300);
    background: var(--color-base-200);
    color: color-mix(in srgb, var(--color-base-content) 70%, transparent);
    font: inherit;
    font-size: 12px;
    font-weight: 600;
    cursor: pointer;
    transition:
      color 150ms ease,
      background-color 150ms ease,
      border-color 150ms ease;
  }

  .pref-btn--icon {
    width: 40px;
    padding: 0;
    justify-content: center;
  }

  .pref-btn:hover {
    color: var(--color-base-content);
    background: var(--color-base-100);
    border-color: color-mix(in srgb, var(--color-base-content) 25%, transparent);
  }

  .pref-btn:focus-visible {
    outline: 2px solid var(--accent);
    outline-offset: 2px;
  }

  .pref-btn__code {
    letter-spacing: 0.04em;
  }

  .pref-menu {
    min-width: 220px;
    padding: 6px;
    border-radius: 12px;
    border: 1px solid var(--color-base-300);
    background: var(--color-base-100);
    color: var(--color-base-content);
    box-shadow: 0 12px 32px -12px rgb(16 24 40 / 0.25);
  }

  .pref-menu__title {
    padding: 6px 8px 8px;
    font-size: 11px;
    font-weight: 600;
    letter-spacing: 0.06em;
    text-transform: uppercase;
    color: color-mix(in srgb, var(--color-base-content) 60%, transparent);
  }

  .pref-item {
    display: flex;
    align-items: center;
    gap: 10px;
    width: 100%;
    padding: 8px;
    border: 0;
    border-radius: 8px;
    background: transparent;
    color: inherit;
    font: inherit;
    font-size: 13px;
    text-align: left;
    cursor: pointer;
  }

  .pref-item:hover {
    background: var(--color-base-200);
  }

  .pref-item--on {
    font-weight: 600;
  }

  .pref-item__meta {
    font-size: 12px;
    color: color-mix(in srgb, var(--color-base-content) 60%, transparent);
  }

  .pref-item__check {
    color: var(--accent-text);
  }
</style>
