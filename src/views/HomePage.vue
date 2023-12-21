<template>
  <div class="container mt-5">
    <div class="row">
        <div class="col-4">
            <div class="card">
                <div class="card-header">
                    <h5>Create</h5>
                </div>
                <div class="card-body">
                    <div class="form-group">
                        <form>
                            <label>Name</label>
                            <input type="text" class="form-control mb-3" />
                            <label>Price</label>
                            <input type="number" class="form-control mb-3" />
                            <button class="btn btn-primary">
                                <i class="fas fa-save mr-2" style="font-size: 1.2rem"></i>
                                Save
                            </button>
                        </form>
                    </div>
                </div>
            </div>
        </div>
        <div class="col-8">
            <table class="table table-striped">
                <thead>
                    <tr>
                        <th>Id</th>
                        <th>Name</th>
                        <th>Price</th>
                        <th>Action</th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td>1</td>
                        <td>book</td>
                        <td>150</td>
                        <td class=" d-flex gap-2">
                            <div class="">
                                <button class=" btn btn-sm btn-info"> <i class=" fas fa-edit"></i> <span>Edit</span></button>
                            </div>
                            <div class="">
                                <button class=" ml-3 btn btn-sm btn-danger"> <i class=" fas fa-trash-alt"></i> <span>Delete</span></button>
                            </div>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</div>
</template>

<script>
    export default {
        
    }
</script>

<style lang="scss" scoped>

</style>