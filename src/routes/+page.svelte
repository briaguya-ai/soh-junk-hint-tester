<!-- /*
static const std::unordered_map<std::string, char> colors = { { "w", QM_WHITE },  { "r", QM_RED },   { "g", QM_GREEN },
                                                              { "b", QM_BLUE },   { "c", QM_LBLUE }, { "p", QM_PINK },
                                                              { "y", QM_YELLOW }, { "B", QM_BLACK } };
*/ -->

<script>
    const charColorMap = new Map([
        ["%w", "#FFFFFF"],
        ["%r", "#FF3C3C"],
        ["%g", "#46FF50"],
        ["%b", "#505AFF"],
        ["%c", "#64B4FF"],
        ["%p", "#FF96B4"],
        ["%y", "#E1FF32"],
        ["%B", "#000000"],
    ]);

	const formatHintText = (unformattedText) => {
        const uncoloredBoxes = unformattedText.
            replaceAll('\\','').
            replaceAll('@','LongName').
            split('^').
            map(box => box.split('&'));

        let colorfulBoxes = [];
        let currentColor = charColorMap.get('%w');
        for (let box of uncoloredBoxes) {
            let colorfulBox = [];
            for (let line of box) {
                let colorfulLine = `<span style="color: ${currentColor}">`;
                let split = line.split(/(%[wrgbcpyB])/);
                for (let lineChunk of split) {
                    if (charColorMap.has(lineChunk)) {
                        currentColor = charColorMap.get(lineChunk);
                        colorfulLine += `</span><span style="color: ${currentColor}">`
                    } else {
                        colorfulLine += lineChunk;
                    }
                }
                colorfulLine += '</span>';
                colorfulBox.push(colorfulLine);
            }
            colorfulBoxes.push(colorfulBox);
        }

        return colorfulBoxes;
    }
    
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
                        {@html line}
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