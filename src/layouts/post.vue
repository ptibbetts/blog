<script setup lang="ts">
import postsPage from '~/pages/blog/posts/index.vue'
import { format } from '~/logic/date'
</script>

<template layout="default">
<div>
    <nav>
        <TerminalCommand command="ls -lah .">
            <LsResult type="directory" :updated="postsPage.meta.lastUpdated">
                <router-link :to="postsPage.href">..</router-link>
            </LsResult>
            <LsResult type="file" color="text-white" :updated="$meta.lastUpdated">
                index.md
            </LsResult>
        </TerminalCommand>
    </nav>

    <main>
        <TerminalCommand command="cat index.md">
            <div mb-4>
                <div>
                    ---
                </div>
                <div >
                    <span class="text-cyan">title</span>
                    <span class="text-pink">: </span>
                    <h1 class="inline font-bold text-yellow">{{ $frontmatter.title }}</h1>
                </div>
                <div>
                    <span class="text-cyan">date</span>
                    <span class="text-pink">: </span>
                    <span class="text-orange">{{ format($frontmatter.date) }}</span>
                </div>
                <div v-if="$frontmatter.tags">
                    <span class="text-cyan">tags</span>
                    <span class="text-pink">: </span>
                    <ul class="pl-4">
                        <li v-for="tag in $frontmatter.tags">
                            <span class="text-pink">- </span> 
                            <router-link 
                                :to="`/blog/tags/${tag}`" 
                                class="text-yellow"
                            >
                                {{ tag }}
                            </router-link>
                        </li>
                    </ul>
                </div>
                <div>
                    ---
                </div>
            </div>
            <div class="prose text-white font-sans">
                <slot />
            </div>
        </TerminalCommand>
    </main>
</div>
</template>