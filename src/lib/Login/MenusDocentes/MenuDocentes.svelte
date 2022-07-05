<script>
    import {
        afterUpdate,
        beforeUpdate,
        createEventDispatcher,
        onDestroy,
        onMount,
    } from "svelte";

    import { URL, Docente } from "../../../stores";

    let htmlMenus = "Notas docentes";

    const generarSubMenus = (docente, nivel, numero, asignaturas) => {
        let html = "";

        asignaturas.forEach((asignatura) => {
            html += `
          <li><a class="dropdown-item menuAsignatura" href="#" data-docente="${docente}" data-nivel="${nivel}" data-numero="${numero}" data-asignatura="${asignatura.asignatura}">${asignatura.asignatura}</a></li>
          `;
        });
        return html;
    };
    const generateMenus = async () => {
        let response = await fetch(`${$URL}generarMenu.php`, {
            method: "POST",
            body: JSON.stringify({
                docente: $Docente,
            }),
            headers: {
                "Content-Type": "application/json",
            },
        });
        let DatosDocente = await response.json();
        if (DatosDocente.length === 0) {
            document.getElementById("notas").classList.add("d-none");
            document.querySelector(".descudo").classList.remove("d-none");
        } else {
            let html = `
   
           
               <a class="nav-link dropdown-toggle" href="#" data-bs-toggle="dropdown">  Grados  </a>
               <ul class="dropdown-menu">
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
    };

    afterUpdate(() => {
        generateMenus();
    });
</script>

<main class="mt-6">
    {@html htmlMenus};
</main>

<style>
    .mt-6 {
        margin-top: 6rem;
    }
</style>
