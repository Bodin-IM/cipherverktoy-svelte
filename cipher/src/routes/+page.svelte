<script>
    let text = $state("");
    let key = $state(1);
    let result = $state("");
    let resultPretty = $state("")

    let errorElement = $state()
    let errorTextElement = $state()

    function encrypt(decrypt=false) {
        let key2
        result = ''
        resultPretty = ''
        if (key <= 0) {
            error("key not set or is negative value")
        }

        if (decrypt == true) {
            key2 = -key
            resultPretty = "Dektryptert: "
        } else {
            key2 = key
            resultPretty = 'Kryptert: '
        }

        for (let i = 0; i < text.length; i++) {
            
            let char = text[i];

            // går gjennom store bokstaver
            if (char >= 'A' && char <= 'Z') {
                // caesar chiffer
                let code = text.charCodeAt(i);
                char = String.fromCharCode(((code - 65 + key2) % 26) + 65);
            }
            // går gjennom små bokstaver
            else if (char >= 'a' && char <= 'z') {
                // caesar chiffer
                let code = text.charCodeAt(i);
                char = String.fromCharCode(((code - 97 + key2) % 26) + 97);
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


    function error(msg="this is a string") {
        errorElement.style.display = "block"
        errorTextElement.innerHTML = msg
    }
</script>

<div id="popupContainer">
    <div bind:this={errorElement} id="popUpError"><h1>ERROR!</h1> <p bind:this={errorTextElement}>You fucking died, you stupid fucking chud <br>  Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla a consectetur leo, eget convallis risus. Praesent augue lorem, eleifend a quam at, luctus auctor risus. Nullam quis dolor maximus, luctus dui et, laoreet nunc. Proin ac est mollis, dictum ex vel, aliquet nisi. In quis ipsum velit. Phasellus hendrerit efficitur eros, sed rutrum diam. Nunc auctor risus ut faucibus tincidunt.<br>Etiam eget libero eros. In fermentum lorem sem. Etiam maximus mauris ac sodales blandit. Donec vel molestie nisl. Donec congue sem sodales tellus consequat pretium. Phasellus aliquet mattis porttitor. Vivamus non blandit odio, et laoreet massa. Nullam nulla nibh, maximus a nunc at, lobortis ornare purus. Etiam porta consequat ante, quis auctor quam congue quis. Duis aliquam libero id odio ullamcorper fermentum. Nam consequat diam in justo ornare, vitae viverra tortor lacinia. </p></div>
</div>

<section>
    <input type="text" id="cipherInput" bind:value={text} placeholder="Tekst som skal krypteres"> <input bind:value={key} type="number" min=1 max=25> <button onclick={() => encrypt()}>Kryptere</button> <button onclick={() => encrypt(true)}>Dektryptere</button>
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
        display: none;
        opacity: 85%;
        position: fixed;
        background-color: grey;
        border-radius: 6px;
        padding: 1px;
        padding-left: 20px;
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
        display: flex;
        justify-content: center;
        align-items: center;
        left: 0px;
        top: 0px;
        height: 100vh;
        width: 100vw;
        pointer-events: none;
    }
</style>