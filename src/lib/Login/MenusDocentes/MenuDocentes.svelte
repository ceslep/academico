<script>
    import {
        afterUpdate,
        beforeUpdate,
        createEventDispatcher,
        onDestroy,
        onMount,
    } from "svelte";
    import { stop_propagation } from "svelte/internal";

    import { URL, Docente } from "../../../stores";
    import SubMenu from "./SubMenu.svelte";

    let htmlMenus = "Notas docentes";
    let DatosDocente = [];

    /* const generarSubMenus = (docente, nivel, numero, asignaturas) => {
        let html = "";

        asignaturas.forEach((asignatura) => {
            html += `
          <li><a class="dropdown-item menuAsignatura" href="#" data-docente="${docente}" data-nivel="${nivel}" data-numero="${numero}" data-asignatura="${asignatura.asignatura}">${asignatura.asignatura}</a></li>
          `;
        });
        return html;
    };
    const generateMenus = async () => {
                if (DatosDocente.length === 0) {
            document.getElementById("notas").classList.add("d-none");
            document.querySelector(".descudo").classList.remove("d-none");
        } else {
            let html = `
   
           
               <a class="nav-link dropdown-toggle" href="#" data-bs-toggle="dropdown">  Grados  </a>
               <ul class="dropdown-menu" bind:this={value}>
`;
            DatosDocente.forEach((dato) => {
                html += `
                    <li><a class="dropdown-item" href="#">Grado ${
                        dato.grado
                    } <span class="float-end">&rtrif;</span></a>
                        <ul class="submenu dropdown-menu">
                            ${generarSubMenus(
                                $Docente,
                                dato.nivel,
                                dato.numero,
                                dato.asignaturas
                            )}
                        </ul>
                    </li>
                
      
        `;
            });
            html += "</ul>";

            htmlMenus = html;
        }
    }; */

    afterUpdate(async () => {
        let response = await fetch(`${$URL}generarMenu.php`, {
            method: "POST",
            body: JSON.stringify({
                docente: $Docente,
            }),
            headers: {
                "Content-Type": "application/json",
            },
        });
        DatosDocente = await response.json();
    });
</script>

<main class="mt-6">
    <a class="nav-link dropdown-toggle" href="#!" data-bs-toggle="dropdown">
        Grados
    </a>
    <ul class="dropdown-menu">
        {#each DatosDocente as dato}
            <li>
                <a
                    class="dropdown-item"
                    href="#!"
                    on:click={(e) => {
                        e.stopPropagation();
                        console.log(e.target)
                    }}
                    >Grado {dato.grado}
                    <span class="float-end">&rtrif;</span></a
                >
                <ul
                    class="submenu dropdown-menu"
                    on:click={(e) => {
                        console.log(e.target);
                    }}
                >
                    <SubMenu
                        docente={$Docente}
                        nivel={dato.nivel}
                        numero={dato.numero}
                        asignaturas={dato.asignaturas}
                    />
                </ul>
            </li>
        {/each}
    </ul>
</main>

<style>
    .mt-6 {
        margin-top: 6rem;
    }
</style>
