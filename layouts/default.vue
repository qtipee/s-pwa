<template>
    <v-app dark>

        <v-navigation-drawer v-if="$auth.loggedIn"
            v-model="drawer"
            clipped
            fixed
            app
        >
            <v-list>
                <v-list-item
                    v-for="(item, i) in navItems"
                    :key="i"
                    :to="item.to"
                    router
                    exact
                >
                    <v-list-item-action>
                        <v-icon>{{ item.icon }}</v-icon>
                    </v-list-item-action>
                    <v-list-item-content>
                        <v-list-item-title v-text="item.title" />
                    </v-list-item-content>
                </v-list-item>
            </v-list>
        </v-navigation-drawer>

    <v-app-bar
        clipped-left
        fixed
        app
    >
        <v-app-bar-nav-icon  v-if="$auth.loggedIn" @click.stop="drawer = !drawer" />
        <v-toolbar-title v-text="title" />
    </v-app-bar>

    <v-content>
        <v-container>

            <v-breadcrumbs :items="breadcrumbsItems">
                <template v-slot:divider>
                    <v-icon>mdi-chevron-right</v-icon>
                </template>
            </v-breadcrumbs>

            <nuxt />

        </v-container>
    </v-content>

    <v-footer
    fixed
    app
    >
        <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>

    </v-app>
</template>

<script>
export default {

    data () {
        return {
            drawer: false,  // Hidden by default
            // TODO: async
            navItems: [
            {
                icon: 'mdi-apps',
                title: 'Home',
                to: '/'
            },
            ],
            title: 'S-PWA',
            // TODO: async
            breadcrumbsItems: [
                {
                    text: 'Home',
                    disabled: false,
                    href: '/',
                },
            ],
        }
    }
}
</script>
