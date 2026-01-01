<script>
  import { data, handle, handleUrl } from '../config'

  let sites = [...data]
  let keyword = ""

  const search = () => {
    sites = [...data].filter((one) => {
      return one.keyword.indexOf(keyword) > -1
        || one.name.indexOf(keyword) > -1
        || one.category.some((one) => one.indexOf(keyword) > -1)
    })
  }

  const handleKeyDown= (e) => {
    if (e.code === 'Enter' && sites[0]) {
      window.open(sites[0].url, '_blank', 'noreferrer')
    }
  }

  const handleEsc = (e) => {
    if (e.code === 'Escape') {
      keyword = ""
      sites = [...data]
      document.getElementById('keyword').focus()
    }
  }

  const highlightMatches = (name, keyword) => {
    if (!keyword) return name

    const keywordLower = keyword.toLowerCase()
    let keywordIndex = 0
    let result = ''

    for (let i = 0; i < name.length; i++) {
      const char = name[i]
      if (keywordIndex < keywordLower.length &&
          char.toLowerCase() === keywordLower[keywordIndex]) {
        result += `<mark class="bg-yellow-200">${char}</mark>`
        keywordIndex++
      } else {
        result += char
      }
    }

    return result
  }
</script>

<svelte:window on:keydown={handleEsc}/>

<main class="mx-auto w-96 font-mono">
  <header>
    <h1 class="text-center text-2xl my-2 italic">Swooooosh</h1>
  </header>
  <input type="text" name="keyword" id="keyword" class="block w-96 mx-auto mb-4 font-monot border p-1 text-center rounded" autofocus bind:value={keyword} on:input={search} on:keydown={handleKeyDown}>
  {#if sites.length > 0}
    <ul class="border p-4 mb-2 rounded">
      {#each sites as site}
        <li class="flex">
          <a
            href="{site.url}"
            target="_blank"
            rel="noreferrer"
            class="flex-1 hover:text-[rgb(200,100,100)]"
          >{@html highlightMatches(site.name, keyword)}</a>
        </li>
      {/each}
    </ul>
  {/if}
</main>

<p class="w-96 mx-auto text-center">built with <span class="text-red-600">&#9829;</span> by <a href="{handleUrl}">{handle}</a></p>
