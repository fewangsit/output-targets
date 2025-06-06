# @wangs-ui/vue-output-target

Stencil can generate Vue component wrappers for your web components. This allows your Stencil components to be used within a Vue application.

For a detailed guide on how to add the vue output target to a project, visit: https://stenciljs.com/docs/vue.

## Installation

```bash
npm install @wangs-ui/vue-output-target
```

### Usage

In your `stencil.config.ts` add the following configuration to the `outputTargets` section:

```ts
import { Config } from '@stencil/core';
import { vueOutputTarget } from '@wangs-ui/vue-output-target';

export const config: Config = {
  namespace: 'demo',
  outputTargets: [
    vueOutputTarget({
      componentCorePackage: 'component-library',
      proxiesFile: '../component-library-vue/src/components.ts',
    }),
    {
      type: 'dist',
      esmLoaderPath: '../loader',
    },
  ],
};
```

## Config Options

| Property                      | Description                                                                                                                                                                                                                                                                                                                      |
| ----------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `componentCorePackage`        | The NPM package name of your Stencil component library. This package is used as a dependency for your Vue wrappers.                                                                                                                                                                                                              |
| `outDir`                 | The output file of all the component wrappers generated by the output target. This file path should point to a location within your Vue library/project.                                                                                                                                                                         |
| `excludeComponents`           | An array of tag names to exclude from generating component wrappers for. This is helpful when have a custom framework implementation of a specific component or need to extend the base component wrapper behavior.                                                                                                              |
| `componentModels`             | This is an array of `ComponentModelConfig` objects for components that should be integrated with `v-model`.                                                                                                                                                                                                                      |
| `loaderDir`                   | This is the path to where the `defineCustomElements` function exists in your built project. If `loaderDir` is not provided, the `/dist/loader` directory will be used.                                                                                                                                                           |
| `includePolyfills`            | If `true`, polyfills will automatically be imported and the `applyPolyfills` function will be called in your proxies file. This can only be used when lazy loading Web Components and will not work when `includeImportCustomElements` is `true`.                                                                                |
| `includeDefineCustomElements` | If `true`, all Web Components will automatically be registered with the Custom Elements Registry. This can only be used when lazy loading Web Components and will not work when `includeImportCustomElements` is `true`.                                                                                                         |
| `includeImportCustomElements` | If `true`, the output target will import the custom element instance and register it with the Custom Elements Registry when the component is imported inside of a user's app. This can only be used with the [Custom Elements Bundle](https://stenciljs.com/docs/custom-elements) and will not work with lazy loaded components. |
| `customElementsDir`           | This is the directory where the custom elements are imported from when using the [Custom Elements Bundle](https://stenciljs.com/docs/custom-elements). Defaults to the `components` directory. Only applies when `includeImportCustomElements` is `true`.                                                                        |

## Interfaces

### ComponentModelConfig

The interface used for `componentModels`.

```typescript
export interface ComponentModelConfig {
  /**
   * An array of element names that
   * should have v-model integration.
   */
  elements: string | string[];

  /**
   * The event emitted from the Web Component
   * that should trigger a `v-model` update.
   */
  event: string;

  /**
   * The Web Component property that the value
   * of the `v-model` reference is based off.
   */
  targetAttr: string;
}
```

Example usage within `stencil.config.ts`:

```typescript
vueOutputTarget({
  ...,
  componentModels: [
    {
      elements: ['my-input', 'my-textarea'],
      event: 'on-change',
      targetAttr: 'value'
    }
  ]
})
```
