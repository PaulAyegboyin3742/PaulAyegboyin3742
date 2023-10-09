<!DOCTYPE html>
<html lang="en-us">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Shopping List</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="/index.css" />
  </head>
  <body class="bg-gray-400">
    <h1 class="text-center text-3xl font-bold py-10">My ShopCart List</h1>

    <div class="flex item-center justify-center flex-col mx-auto max-w-sm">
      <label class="text-lg" for="item">Enter a new item</label>
      <input
        type="text"
        name="item"
        id="item"
        class="block w-full mt-2 bg-white placeholder:text-gray-200 text-sm px-4 py-3.5 rounded-xl shadow-sm border border-transparent focus:border-black focus:outline-none"
      />
      <button class="bg-blue-500 text-white py-3 rounded-xl mt-4">
        Add Item
      </button>
    </div>

    <ul class="mx-auto max-w-sm pt-6"></ul>

    <div id="item-count" class="text-center text-xl font-bold mb-4"></div>

    <script>
      const list = document.querySelector("ul");
      const input = document.querySelector("input");
      const button = document.querySelector("button");
      const itemCountElement = document.getElementById("item-count");
      const savedList = JSON.parse(localStorage.getItem("shoppingList")) || [];

      function saveList() {
        localStorage.setItem("shoppingList", JSON.stringify(savedList));
      }

      function updateItemCount() {
        itemCountElement.textContent = `You have ${list.children.length} items in your list`;
      }

      function addItemToList(myItem) {
        const listItem = document.createElement("li");
        const listText = document.createElement("span");
        const listBtn = document.createElement("button");
        const editBtn = document.createElement("button");

        listItem.appendChild(listText);
        listText.textContent = myItem;
        listItem.appendChild(listBtn);
        listBtn.textContent = "Delete";
        listItem.appendChild(editBtn);
        editBtn.textContent = "Edit";
        list.appendChild(listItem);

        listBtn.addEventListener("click", () => {
          const confirmation = confirm(
            "Are you sure you want to delete this item?"
          );

          if (confirmation) {
            list.removeChild(listItem);
            savedList.splice(savedList.indexOf(myItem), 1);
            saveList();
            updateItemCount();
          }
        });

        editBtn.addEventListener("click", () => {
          const newText = prompt("Enter new text:");
          if (newText !== null && newText !== "") {
            listText.textContent = newText;
            savedList[savedList.indexOf(myItem)] = newText;
            saveList();
          }
        });

        updateItemCount();
        input.focus();
      }

      button.addEventListener("click", () => {
        const myItem = input.value.trim();
        if (myItem !== "") {
          addItemToList(myItem);
          input.value = "";
        }
      });

      input.addEventListener("keydown", (event) => {
        if (event.key === "Enter") {
          event.preventDefault();
          button.click();
        }
      });

      savedList.forEach((item) => {
        addItemToList(item);
      });
    </script>
  </body>
</html>
