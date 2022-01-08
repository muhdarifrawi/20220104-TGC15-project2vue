<template>
  <div class="container">
    <h1>Product Entry</h1>
    <div class="form-floating mb-2">
      <input
        type="date"
        name="entry-date"
        id="entry-date"
        class="form-control"
        placeholder="John Doe"
      />
      <label for="entry-date">Entry Date</label>
    </div>

    <div class="form-floating mb-2">
      <input
        type="text"
        name="user"
        id="user"
        class="form-control"
        placeholder="John Doe"
      />
      <label for="user">User</label>
    </div>

    <div class="form-floating mb-2">
      <input
        type="text"
        name="item-name"
        id="item-name"
        class="form-control"
        placeholder="John Doe"
      />
      <label for="entry-date">Item Name</label>
    </div>

    <div class="form-floating mb-2">
      <select class="form-select" id="category" aria-label="category label">
        <option selected>Choose a category</option>
        <option value="1">One</option>
        <option value="2">Two</option>
        <option value="3">Three</option>
      </select>
      <label for="floatingSelect">Category</label>
    </div>

    <div class="form-floating mb-2">
      <textarea
        class="form-control"
        placeholder="Fill in description"
        id="item-description"
      ></textarea>
      <label for="item-description">Item Description</label>
    </div>
  </div>
</template>

<script>
export default {
  name: "AddListing",
};
</script>

<style>
#item-description {
  height: 150px;
  resize:none;
}
</style>