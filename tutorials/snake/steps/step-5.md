# Step 5

## Wall collision and game over conditions

If the snake hits a wall, the game ends. Similarly if the snake tries to eat a part of its body, the game also ends.

Remember that we have a sequence that represents the whole snake body. The last element of that sequence is the head.

To test whether the snake is going outside the bounds of the play space, we just need to compare the `x` (first coordinate) and `y` (second coordinate) of the head with the number of columns (resp. with the number of rows). We need to do that before attempting to render as it otherwise would go out of bounds and produce an error (and since we don't handle that error, crash the game). We will use a boolean value (`true` or `false`) to mark when the game is over and avoid updating the gird in such case.

=== "EDN"

    ```clojure linenums="1"
    ; get the head coordinates
    .snake (RTake 0) >= .head

    ; snake hits a wall?
    (When (-> .head (Take 0) (IsLess 0) (Or) (Take 0) (IsMoreEqual grid-cols)
              (Or)
              .head (Take 1) (IsLess 0) (Or) (Take 1) (IsMoreEqual grid-rows))
          (-> true > .game-over))
    ```

To check whether the snake is eating is body, we use the fact that the head is the last element of the snake sequence. If we find another body cell that have the same coordinates while having a different index in the sequence, it means we have an overlap and the poor snake is in pain.

=== "EDN"

    ```clojure linenums="1"
    ; get the index of the head (which is count - 1)
    (Count .snake) (Math.Subtract 1) >= .head-idx

    ; snake eats its own body?
    (When (-> .snake (IndexOf .head) (IsNot .head-idx))
          (-> true > .game-over))
    ```

## A few graphical changes

Now that have conditions to end the game, we can a bit more logic to display the final score which in our case will be the length of the snake.

=== "EDN"

    ??? info inline end
        [`(GUI.Text)`](https://docs.fragcolor.xyz/blocks/GUI/Text/) has a few optional parameters:

        - `:Format`: where `{}` is replaced by the input value.
        - `:Color`: to use a different color.

        `color` is a built-in types that represents a RGBA color, where each component is a value in the `[0, 255]` range.

    ```clojure linenums="1"
    (GUI.Window
      ; some lines omitted
      (-> .grid (render)
          (GUI.Separator)
          (When (-> .game-over)
                (-> "GAME OVER" (GUI.Text :Color (color 255 0 0 255))
                    (Count .snake) (GUI.Text :Format "Final score: {}")))))
    ```

We will also change the background color of the play space. We could change the background color of the whole window, but then the area with the **GAME OVER** text  would also share that color. Instead, we will create a new area for the game itself. To do so we can use a child window.

=== "EDN"

    ??? info inline end
        [`(GUI.Style)`](https://docs.fragcolor.xyz/blocks/GUI/Style/) lets modify a UI style setting identified by the `GuiStyle` enum.

    ```clojure linenums="1"
    (color 90 165 80 255) (GUI.Style GuiStyle.ChildBgColor)
    (GUI.ChildWindow :Height 240 :Contents (-> .grid (render)))
    ```

We will also add a small menu, to cleanly restart or exit the game.

=== "EDN"

    ??? info inline end
        [`(GUI.Menu)`](https://docs.fragcolor.xyz/blocks/GUI/Menu/) contains one or more [`(GUI.MenuItem)`](https://docs.fragcolor.xyz/blocks/GUI/MenuItem/) and is hosted in a [`(GUI.MenuBar)`](https://docs.fragcolor.xyz/blocks/GUI/MenuBar/).

    ```clojure linenums="1"
    (defblocks menus []
      (GUI.MenuBar
       (-> (GUI.Menu
            "File" :Contents
            (-> (GUI.MenuItem "New game" :Shortcut "Space" :Action (initialize))
                (GUI.Separator)
                (GUI.MenuItem "Quit" :Shortcut "Alt+F4" :Action (Stop)))))))
    ```

To display the menu we need to add the `GuiWindowFlags.MenuBar` to the sequence of flags given to the window.

That's it! Congratulations on completing the snake tutorial.

The complete code is available in the [full game](../full-game/index.md) section.

--8<-- "includes/license.md"