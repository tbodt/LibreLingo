<script lang="typescript" context="module">
  export async function getMarkDownData(markdownModule) {
      const remark = await require("remark")
      const markdown = await import("remark-parse")
      const html = await import("rehype-stringify")
      const remark2rehype = await import("remark-rehype")
      const format = await import("rehype-format")

      return (await remark()
          .use(markdown.default, { gfm: true, commonmark: true })
          .use(remark2rehype.default)
          .use(format.default)
          .use(html.default)
          .process(markdownModule.default)).contents
  }
</script>

<script lang="typescript">
  import NavBar from "../components/NavBar.svelte"
  import Content from "lluis/Content.svelte"

  export let readmeHTML: string
  export let title: string | null
  export let description: string | null = null
</script>

<svelte:head>
  <title>{title}</title>
  {#if description}
    <meta name="description" content="{description}" />
  {/if}
</svelte:head>

<NavBar />

<section class="hero is-primary">
  <div class="hero-body">
    <div class="container">
      <h1 class="title">{title}</h1>
      <div class="section box">
        <slot />
        <Content>
          {@html readmeHTML}
        </Content>
      </div>
    </div>
  </div>
</section>
