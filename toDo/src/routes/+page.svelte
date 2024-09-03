<script lang='ts'>
    import { Input, Button } from 'flowbite-svelte';
    import { Card } from 'flowbite-svelte';
    import { CheckCircleSolid, EditSolid } from 'flowbite-svelte-icons';
    type toDo = {
        id: string;
        name: string;
    }
    let addInputValue = ''
    let editInputValue = ''
    let selectedToDoId = ''
    let editPlaceholder = ''
    let editMode = false
    let toDos: toDo[] = []

    const addToDo = () => {
        toDos = [...toDos, {
            id: self.crypto.randomUUID(),
            name: addInputValue,
        }]
        addInputValue = ''
    }
    const completeToDo = (id: string) => {
        toDos = [...toDos.filter((toDo) => toDo.id !== id)]
    }
    const enableEditMode = (id: string) => {
        selectedToDoId = id
        toDos.forEach((toDo) => {
            if (toDo.id === id) {
                editPlaceholder = toDo.name
            }
        })
        editMode = !editMode
    }
    const editToDo = () => {
        toDos = [...toDos.map((toDo) => {
            if (toDo.id === selectedToDoId) {
                return {
                    id: toDo.id,
                    name: editInputValue,
                    complete: false
                }
            }
            return toDo
        })]
        editInputValue = ''
        editMode = !editMode
    }
</script>

<div class="flex justify-center items-center h-screen">
<Card>
    {#if !editMode}
    <h1 class='mb-2'>Add</h1>
    <Input bind:value={addInputValue} id="add" placeholder="Add toDo Item" size="lg">
        <Button on:click={() => addToDo()} slot="right" size="sm" type="submit">Add</Button>
    </Input>
    {:else}
    <h1 class='mb-2'>Edit</h1>
    <Input bind:value={editInputValue} id="edit" bind:placeholder={editPlaceholder} size="lg">
        <div slot="right">
        <Button on:click={() => editToDo()} size="sm" type="submit">Save</Button>
        <Button on:click={() => editMode = !editMode} size="sm" type="submit">Cancel</Button>
        </div>
    </Input>
    {/if}
    <div class='m-2'>
        {#each toDos as toDo (toDo.id)}
            <div class='m-2 p-2 flex justify-between'>
                <h1>{toDo.name}</h1>
                <div class='flex'>
                <button on:click={() => enableEditMode(toDo.id)}><EditSolid class='hover:fill-blue-500' /></button>
                <button on:click={() => completeToDo(toDo.id)}><CheckCircleSolid class='hover:fill-green-500' /></button>
                </div>
            </div>
        {/each}
    </div>
</Card>
</div>