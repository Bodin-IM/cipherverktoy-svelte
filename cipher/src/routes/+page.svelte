<script>
    let textCaesar = $state("");
    let keyCaesar = $state(1);

    let textVignere = $state("")
    let keyVignere = $state("")

    let cipherTypes = ["Caesar", "Vignere"]
    let selectedCipher = $state(cipherTypes[0])

    let result = $state("");
    let resultPretty = $state("")

    // lmfao
    let alphabet = ["A", "B", "C", "D", "E", "F", "G", "H", "I", "J", "K", "L", "M", "N", "O", "P", "Q", "R", "S", "T", "U", "V", "W", "X", "Y", "Z"]

    let errorElement = $state()
    let errorTextElement = $state()
    let errorContainerElement = $state()

    // Encryption //
    
    function encryptCaesar(decrypt=false) {
        let key
        result = ''
        resultPretty = ''
        if (keyCaesar <= 0) {
            error("key not set or is negative value")
            return
        }
        if (isOnlyLetters(textCaesar) == false) {
            error("Text contains characters other than letters (A-Z)")
            return
        }
        
        if (decrypt == true) {
            key = -keyCaesar
            resultPretty = "Dektryptert: "
        } else {
            key = keyCaesar
            resultPretty = 'Kryptert: '
        }
        
        for (let i = 0; i < textCaesar.length; i++) {
            
            let char = textCaesar[i];
            
            // går gjennom store bokstaver
            if (char >= 'A' && char <= 'Z') {
                // caesar chiffer
                let code = textCaesar.charCodeAt(i);
                char = String.fromCharCode(((code - 65 + key) % 26) + 65);
            }
            // går gjennom små bokstaver
            else if (char >= 'a' && char <= 'z') {
                // caesar chiffer
                let code = textCaesar.charCodeAt(i);
                char = String.fromCharCode(((code - 97 + key) % 26) + 97);
            }
            // når æ dekrypterte 'z', ble det '`', dette fikser det! :D
            char = char.replace("`", "z");
            
            // det andre forblir uendret
            result += char;
        }
        // bedre visning
        resultPretty += result;
        
        // trengs ikke egt
        return result;
    }

    // WIP
    function encryptVignere() {
        let text = textVignere
        let key = keyVignere
        
        var s = new Array();
	    for (var i = 0; i < key.length; i++) {
		    s[i] = alphabet.indexOf(key.toUpperCase.charAt(i)) +1;
	    }
	    
        resultPretty = "fuckin array: " + s.toString()
    }


    
    // Number check

    function isOnlyLetters(str="blue") {
        var code, i, len;
        let charCount = 0
        
        for (i = 0, len = str.length; i < len; i++) {
            
            //str.char
            code = str.charCodeAt(i) 
            console.log(code)
            console.log(charCount)
            // går gjennom hvis det ikke er mellomrom
            if (code != 32) {
                charCount++
                console.log(charCount)
                // sjekk om bokstav ikke er i alfabet
                if (!(code > 64 && code < 91) && // upper alpha (A-Z)
                !(code > 96 && code < 123)) { // lower alpha (a-z)
                    // hvis ikke i alfabet
                    return false;
                } 
                
            }
        }
        // caller error om det er tomt
        if (charCount >= 1) {
            return true;
        } else {
            error("du må ha bokstav")
        }
    };

    // Error messages //


    function error(msg="this is a string") {
        errorContainerElement.style.display = "flex";
        errorTextElement.innerHTML = msg;
        resultPretty = "ERROR!";
    }

    function hideError() {
        errorContainerElement.style.display = "none";
        errorTextElement.innerHTML = "no error";
    }
</script>

<a bind:this={errorContainerElement} href="?balls" onclick={hideError} id="popupContainer">
    <div bind:this={errorElement} id="popUpError"><h1>ERROR!</h1> <p bind:this={errorTextElement}>You fucking died, you stupid fucking chud <br>  Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla a consectetur leo, eget convallis risus. Praesent augue lorem, eleifend a quam at, luctus auctor risus. Nullam quis dolor maximus, luctus dui et, laoreet nunc. Proin ac est mollis, dictum ex vel, aliquet nisi. In quis ipsum velit. Phasellus hendrerit efficitur eros, sed rutrum diam. Nunc auctor risus ut faucibus tincidunt.<br>Etiam eget libero eros. In fermentum lorem sem. Etiam maximus mauris ac sodales blandit. Donec vel molestie nisl. Donec congue sem sodales tellus consequat pretium. Phasellus aliquet mattis porttitor. Vivamus non blandit odio, et laoreet massa. Nullam nulla nibh, maximus a nunc at, lobortis ornare purus. Etiam porta consequat ante, quis auctor quam congue quis. Duis aliquam libero id odio ullamcorper fermentum. Nam consequat diam in justo ornare, vitae viverra tortor lacinia. </p></div>
    
</a>

<section>
    <select bind:value={selectedCipher}>
        <option value={cipherTypes[0]}>{cipherTypes[0]}</option>
        <option value={cipherTypes[1]}>{cipherTypes[1]}</option>
    </select>
    {#if selectedCipher == cipherTypes[0]}
        <input type="text" id="cipherInput" bind:value={textCaesar} placeholder="Tekst som skal krypteres"> <input bind:value={keyCaesar} type="number" min=1 max=25> <button onclick={() => encryptCaesar()}>Kryptere</button> <button onclick={() => encryptCaesar(true)}>Dektryptere</button>
    {:else if selectedCipher == cipherTypes[1]}
        <input type="text" id="cipherInput" bind:value={textVignere} placeholder="Tekst som skal krypteres"> <input bind:value={keyVignere} type="text" placeholder="Key" style="width: 11%;"> <button onclick={() => encryptVignere()}>Kryptere</button> <button onclick={() => encryptVignere()}>Dektryptere</button>
    {:else}
        <p>ts not implemented yet lmfao</p>
    {/if}
</section>

<section>
    {#key result}
        <h1>{resultPretty}</h1>
    {/key}
</section>

<section>
    <button onclick={() => error("this error was triggered by the user")}>Trigger error</button>
</section>




<style>
    section {
        margin-bottom: 20px;
        padding: 20px;
        background: white;
        border: 1px solid #ddd;
        border-radius: 6px;
    }

    #popUpError {
        display: block;
        opacity: 85%;
        position: fixed;
        background-color: grey;
        border-radius: 6px;
        padding: 1px;
        padding-left: 20px;
        padding-right: 20px;
        pointer-events: none;
        

        h1 {
            color: red;
            text-align: center;
        }
        p {
            text-align: center;
        }
    }

    #popupContainer {
        position: fixed;
        display: none;
        justify-content: center;
        align-items: center;
        left: 0px;
        top: 0px;
        height: 100vh;
        width: 100vw;
        color: black;

        
    }
</style>