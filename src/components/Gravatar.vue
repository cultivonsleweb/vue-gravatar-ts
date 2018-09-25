<template>
  <img :src="url()" :alt="alt" @load="onLoad" @error="onError" />
</template>

<script lang="ts">
import { Component, Vue, Prop } from 'vue-property-decorator';
import md5 from 'md5';

const GravatarProps = Vue.extend({
  props: {
    email: {
      type: String,
      default: '',
    },
    hash: {
      type: String,
      default: '',
    },
    size: {
      type: Number,
      default: 80,
    },
    defaultImg: {
      type: String,
      default: 'retro',
    },
    rating: {
      type: String,
      default: 'g',
    },
    alt: {
      type: String,
      default: 'Avatar',
    },
  },
});

@Component
export default class Gravatar extends GravatarProps {
  public computed(): void {
    this.url();
  }

  public onLoad(...args: string[]): void {
    this.$emit('load', ...args);
  }

  public onError(...args: string[]): void {
    this.$emit('error', ...args);
  }

  public url($event: any = null): string {
    const img = [
      '//www.gravatar.com/avatar/',
      this.hash || md5(this.email.trim().toLowerCase()),
      `?s=${this.size}`,
      `&d=${this.defaultImg}`,
      `&r=${this.rating}`,
    ];
    return img.join('');
  }
}
</script>
