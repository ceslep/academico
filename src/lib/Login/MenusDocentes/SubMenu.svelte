<script>
    export let docente;
    export let nivel;
    export let numero;
    export let asignaturas;
</script>

{#each asignaturas as asignatura}
<li>
    <a
        class="dropdown-item menuAsignatura"
        href="#!"
        data-docente={docente}
        data-nivel={nivel}
        data-numero={numero}
        data-asignatura={asignatura.asignatura}>{asignatura.asignatura}</a
    >
</li>
{/each}