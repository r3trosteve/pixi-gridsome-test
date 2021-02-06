<template>
  <v-app>
    <div>
      <v-app-bar
        fixed
      >
        <v-btn icon :to="$url('/')" title="Home">
          <v-icon>mdi-home</v-icon>
        </v-btn>
        <v-toolbar-title>{{ $static.metadata.siteName }}</v-toolbar-title>
        <v-spacer />

        <v-btn :to="$url('/')">
          Home
        </v-btn>
        <v-btn :to="$url('/about')">
          About
        </v-btn>
        <v-btn @click="">
          Stats
        </v-btn>
      </v-app-bar>
    </div>
      <div>
        <slot />
      </div>
  </v-app>
</template>

<static-query>
  query {
    metadata {
      siteName
    }
  }
</static-query>