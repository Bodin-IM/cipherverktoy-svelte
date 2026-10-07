<script>
    let text = $state("");
    let key = $state(1);
    let result = $state("");

    function encrypt(decrypt=false) {
        let key2
        result = ''
        if (decrypt == true) {
            key2 = -key
        } else {
            key2 = key
        }
        for (let i = 0; i < text.length; i++) {
            
            let char = text[i];

            // sjekk om det er stor bokstav
            if (char >= 'A' && char <= 'Z') {
                // caesar chiffer
                let code = text.charCodeAt(i);
                char = String.fromCharCode(((code - 65 + key2) % 26) + 65);
            }
            // sjekk om der er liten bokstav
            else if (char >= 'a' && char <= 'z') {
                // caesar chiffer
                let code = text.charCodeAt(i);
                char = String.fromCharCode(((code - 97 + key2) % 26) + 97);
            }
            // det andre forblir uendret

            result += char;
        }

        return result;
    }
</script>


<section>
    <input type="text" id="cipherInput" bind:value={text} placeholder="Tekst som skal krypteres"> <input bind:value={key} type="number" min=1 max=25> <button onclick={() => encrypt()}>Kryptere</button> <button onclick={() => encrypt(true)}>Dektryptere</button>
</section>

<section>
    {#key result}
        <h1>Resultat: {result}</h1>
    {/key}
</section>




<style>
    section {
        margin-bottom: 20px;
        padding: 20px;
        background: white;
        border: 1px solid #ddd;
        border-radius: 6px;
    }
</style>