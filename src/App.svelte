<script>
	let text, arraytext, textarea
  let copied = false

	function convert() {		
		let textHTML = textarea.innerHTML

		arraytext = textHTML.split("<br>")
		arraytext = arraytext.map((d, i) => {
			let a = d.split(/[<>]/)
			a = a.filter(d => d.length > 0)
			a.map((d,i) => {
				if (d.includes("italic")) {
					a[i] = "<em>"
					a[i+2] = "</em>"
				}
			})
      a.map((d,i) => {
				if (d.includes("a href")) {
          d = d.replace("a href=", "")
          d = d.replace(` style="text-decoration:none;"`, "")
          d = d.replace(d[0], "")
          d = d.replace(d[d.length-1], "")
					a[i] = `<a target='_blank' class='link-article' href='${d}'>`
				}
        if (d.includes("/a")) {
					a[i] = "</a>"
				}
			})
			a = a.filter(d => d != "/span" && d != "/p" && d != "/div")
			a = a.filter(d => !d.includes("p dir"))
			a = a.filter(d => !d.includes("span style"))
			a = a.filter(d => !d.includes("span id"))
      a = a.filter(d => !d.includes("div"))
      a = a.map(aa => aa.replace("&nbsp;", ""))
      a = a.join("")
			return a
		})
    arraytext = arraytext.filter(d => d.length != 0)
	let a = []
	arraytext.forEach((d) => {
		let c
		let b = d.indexOf("b style=")
		if (b != -1) {
			c = d.slice(0, b).replace("/b","")
		} else {
			c = d.replace("/b", "")
		}
		a.push(c)
	})
	arraytext = a
	}

  function copy() {
    let full = `"` + arraytext.join(`", \n\n"`) + `"`
    navigator.clipboard.writeText(full)
    copied = true
    setTimeout(() => {
      copied = false
    }, 1000)
  }
</script>
<!-- <h1>THIS IS A SVELTE 4 TEST PAGE</h1> -->
<!-- <img src="/images/svelte.svg" alt="test"> -->
<div contenteditable="true" class="textarea" bind:this={textarea}>
</div>
<br>
<button on:click={convert}>Submit</button>
<button on:click={copy}>{copied ? "Copied!" : "Copy"}</button>
{#if arraytext}
	{#each arraytext as t}
		<p>"{t.replace("&nbsp;", "")}",</p>
	{/each}
{/if}

<style>
	.textarea {
		width:500px;
		height:200px;
		border:2px solid black;
		border-radius:0.5rem;
		padding:0.5rem;
		overflow:scroll;
    text-align: left;
	}
	p {
		width:500px;
    text-align: left;
	}
</style>

