<script>
	const formatHintText = (unformattedText) => (
        unformattedText.
            replaceAll('\\','').
            replaceAll('@','LongName').
            split('^').
            map(box => box.split('&'))
    );
    
    let enteredHintText = '';
    $: hintTextBoxes = formatHintText(enteredHintText);
</script>

<div class="everythingContainer">
    <h1>SoH Junk Hint Tester</h1>
    <textarea bind:value={enteredHintText} placeholder="hint text"/>
    {#if enteredHintText}
        <div class="hintPreview">
            {#each hintTextBoxes as box}
                <div class="hintTextBox">
                    {#each box as line}
                    <div class="hintTextLine">
                        {line}
                    </div>
                    {/each}
                </div>
            {/each}
        </div>
    {/if}
</div>

<style>
    .everythingContainer {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
    }

    textarea {
        font-size: larger;
        width: 40ch;
        height: 5em;
    }

    .hintTextBox {
        background-color: rgba(0,0,0,.5);
        width: 38ch;
        padding-left: 5ch;
        margin-top: 1em;
        padding-top: .875em;
        height: 6.5em;
    }

    .hintTextLine {
        white-space: nowrap;
    }
</style>