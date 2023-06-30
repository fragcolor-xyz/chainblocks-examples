# Step 6

## Making the Game Harder - Overview

We are almost towards the end of the tutorial. Wahoo!

In the previous chapter, we learnt how to make coins for Glod to collect.

In this chapter, we will do the opposite. We will make the game more challenging by creating hazards for Glod to avoid!

Poor Glod. 😢

To do this we will be:

1. Drawing a spiked cannonball image on screen.

2. Having it move using variables.

3. Creating a spawning system to randomly spawn a spiked cannonball on screen.

4. Creating a collision system to allow Glod to be hit.

5. Reusing code to create more Spiked Cannonballs by making shards accept arguments.

If you are wondering why the list above seems familiar, it is because the exact same logic and methods were used in the previous chapter.

Let's get to it. 🔥

# Step 6.1

We start off nice and easy by downloading the required images and displaying one of them on the screen.

1. Download spiked cannonball images [here](https://drive.google.com/drive/folders/1qbuwq-DiO0AqiprdQPtm_UeqBG67uN6K?usp=share_link).

Load one image first. To keep our code organized, create a new `initialize-spiked-cannonballs` shard.

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
    ;; ------------ Initialize Spiked CanonBalls ---------------
    (defshards initialize-spiked-canonballs []
      (LoadTexture "GlodImages/SpikeBall/SpikeBall1.png") = .spikeball-1)
    ```

Remember to call your new shard in `main-wire`.

=== "Code Added"

    ```{.clojure .annotate linenums="1"}
    (initialize-spiked-canonballs) ;; (1)
    ```

To draw our loaded image, create a `UI.Area`.

=== "Code Added"

    ```{.clojure .annotate linenums="1"}
    (UI.Area :Position (float2 0 0)
    :Anchor Anchor.Top
    :Contents (->
    .spikeball-1 (UI.Image :Scale (float2 0.15))))
    ```

=== "Full Code So Far"
    
    ```{.clojure .annotate linenums="1"}
    (defshards LoadTexture [name]
      (LoadImage name)
      (GFX.Texture))

    (defshards initialize-character []
      (LoadTexture "GlodImages/Character1_Jumping_Left.png") = .character-jumping-left
      (LoadTexture "GlodImages/Character1_Jumping_Right.png") = .character-jumping-right

      0 >= .character-state
      0 >= .character-direction
      true >= .can-jump

      0.0 >= .X
      620.0 >= .Y
      (float2 .X .Y) >= .character-position
      0.0 >= .character-x-velocity
      0.0 >= .character-y-velocity
      0.0 >= .character-y-acceleration

      ;; ---------- Character Idle Array (Facing Left) ----------
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_1.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_2.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_3.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_4.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_5.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_6.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_7.png") >> .idle-left-image-array

      ;; ---------- Character Idle Array (Facing Right) ----------------
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_1.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_2.png") >> .idle-right-image-array
      (LoadTexture"GlodImages/Character_Idle/Idle_Right/Character1_Idle_3.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_4.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_5.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_6.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_7.png") >> .idle-right-image-array

      0 >= .idle-image-index
      (Count .idle-left-image-array) = .idle-image-index-max
      0.08 = .idle-animation-speed

      ;; -------------- Walking Array (Facing Left) -----------------
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_1.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_2.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_3.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_4.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_5.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_6.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_7.png") >> .walking-left-image-array

      ;; ----------- Walking Array (Facing Right) ---------------
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_1.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_2.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_3.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_4.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_5.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_6.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_7.png") >> .walking-right-image-array

      (Count .walking-left-image-array) = .walking-image-index-max
      0 >= .walking-image-index
      0.08 = .walking-animation-speed) ;; Reduce number to increase animation speed

    ;; --------- Idle Animation Loop ---------
    (defloop idle-animation
      .idle-image-index (Math.Add 1)
      > .idle-image-index
      (When :Predicate (IsMoreEqual .idle-image-index-max)
            :Action (-> 0 > .idle-image-index))
      (Pause .idle-animation-speed))

    ;; -------- Walking Animation Loop --------
    (defloop walking-animation
      .walking-image-index (Math.Add 1)
      > .walking-image-index
      (When :Predicate (IsMoreEqual .walking-image-index-max)
            :Action (-> 0 > .walking-image-index))
      (Pause .walking-animation-speed))

    ;; ---------- character-boundary ------------

    (defshards clamp [var min max]
      var (Max min) (Min max) > var)

    ;; ------------ Character Run Logic ----------------
    (defshards run-logic []
      .X (Math.Add .character-x-velocity)
      > .X

      (float2 .X .Y) > .character-position

      (clamp .X -600.0 600.0))

    ;; ------------ Character gravity-logic ---------------
    (defshards gravity-logic []
      .Y (Math.Add .character-y-velocity)
      > .Y

      .character-y-velocity (Math.Add .character-y-acceleration)
      > .character-y-velocity

      (float2 .X .Y) > .character-position

      (clamp .Y -620.0 620.0)
      .Y
      (When :Predicate (IsMoreEqual 620.0)
            :Action (->
                    0.0 > .character-y-velocity
                    0.0 > .character-y-acceleration
                    true > .can-jump
                    .character-state
                    (When :Predicate (Is 3)
                          :Action (->
                                    0 > .character-state)))))

    ;; ------- Button Inputs ----------
    (defshards button-inputs []
      (Inputs.KeyDown
      :Key "left"
      :Action (->
                (Msg "left")

                .character-state
                (When :Predicate (Is 0)
                      :Action (-> 1 > .character-state))

                0 > .character-direction
                -5.0 > .character-x-velocity))

      (Inputs.KeyDown
      :Key "right"
      :Action (->
                (Msg "right")

                .character-state
                (When :Predicate (Is 0)
                      :Action (-> 2 > .character-state))
                1 > .character-direction
                5.0 > .character-x-velocity))

      (Inputs.KeyDown
      :Key "up"
      :Action (->
                (Msg "up")
                3 > .character-state
                .can-jump
                (When :Predicate (Is true)
                      :Action (->
                              -20.0 > .character-y-velocity
                              1.0 >  .character-y-acceleration
                              false >= .can-jump))))

      (Inputs.KeyUp
      :Key "left"
      :Action (->
                0 > .character-state
                0.0 > .character-x-velocity))

      (Inputs.KeyUp
      :Key "right"
      :Action (->
                0 > .character-state
                0.0 > .character-x-velocity)))

    ;; -------------- Initialize Coin ----------
    (defshards initialize-coin []
      (LoadTexture "GlodImages/Coin/Coin_1.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_2.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_3.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_4.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_5.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_6.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_7.png") >> .coin-image-array
      (Count .coin-image-array) = .coin-image-index-max
      0 >= .coin-image-index
      0.1 = .coin-animation-speed

      ;; ----- Coin 1 ------
      0.0 >= .coinx-1
      0.0 >= .coiny-1
      (float2 .coinx-1 .coiny-1) >= .coin-position-1
      0.0 >= .coin-velocity-1
      0.5 >= .coin-acceleration

      ;; ----- Coin 2 ----
      0.0 >= .coinx-2
      0.0 >= .coiny-2
      (float2 .coinx-2 .coiny-2) >= .coin-position-2
      0.0 >= .coin-velocity-2)

    ;; -------------- Coin Animation ------------------
    (defloop coin-animation
      .coin-image-index (Math.Add 1)
      > .coin-image-index
      (When :Predicate (IsMoreEqual .coin-image-index-max)
            :Action (-> 0 > .coin-image-index))

      (Pause .coin-animation-speed))

    ;; ------------- Coin Gravity ------------------
    (defshards coin-gravity-logic [coiny coinx coin-velocity coin-position]

      coiny (Math.Add coin-velocity)
      > coiny

      coin-velocity (Math.Add .coin-acceleration)
      > coin-velocity

      (float2 coinx coiny) > coin-position)

    ;; ------------- Random Coin ------------------
    (defshards random-coin [coinx coiny coin-velocity coin-position pause-length]
      coinx
      (RandomFloat :Max 1200.0)
      > coinx
      (Math.Subtract 600.0)
      > coinx

      0.0 > coiny
      0.0 > coin-velocity
      (float2 coinx coiny) > coin-position
      (Pause pause-length))

    (defloop random-coin-1
      (random-coin .coinx-1 .coiny-1 .coin-velocity-1 .coin-position-1 1.5))

    (defloop random-coin-2
      (random-coin .coinx-2 .coiny-2 .coin-velocity-2 .coin-position-2 2.5))

    ;; ------------ Initialize Spiked CanonBalls ---------------
    (defshards initialize-spiked-canonballs []
      (LoadTexture "GlodImages/SpikeBall/SpikeBall1.png") = .spikeball-1)


    ;; --------- Game Elements ------------
    (defshards initialize-game-elements []
      0 >= .score
      false >= .scored

      .X (Math.Add 50.0)
      >= .scoringUpper-x-limit
      .X (Math.Subtract 50.0)
      >= .scoringLower-x-limit

      .Y (Math.Add 10.0)
      >= .scoringUpper-y-limit
      .Y (Math.Subtract 10.0)
      >= .scoringLower-y-limit)

    ;; --------- Scoring ----------
    (defshards score-collision [coinx coiny]
      coinx
      (When :Predicate (->
                        (IsLess .scoringUpper-x-limit)
                        (And)
                        coinx (IsMore .scoringLower-x-limit)
                        (And)
                        coiny (IsLess .scoringUpper-y-limit)
                        (And)
                        coiny (IsMore .scoringLower-y-limit)
                        (And)
                        .scored (Is false))
            :Action (->
                    true > .scored
                    (Log "Score: "))))

    (defshards scoring []
      .X (Math.Add 50.0)
      > .scoringUpper-x-limit
      .X (Math.Subtract 50.0)
      > .scoringLower-x-limit

      .Y (Math.Add 10.0)
      > .scoringUpper-y-limit
      .Y (Math.Subtract 10.0)
      > .scoringLower-y-limit

      (score-collision .coinx-1 .coiny-1)
      (score-collision .coinx-2 .coiny-2)

      .scored
      (When :Predicate (Is true)
            :Action (->
                    .score (Math.Add 1)
                    > .score
                    false > .scored)))



    ;; ------ UI Style --------
    (def style
      {:override_text_style "MyStyle"
      :text_styles
      [{:name "MyStyle"
        :size (float 46)
        :family "Monospace"}]
      :visuals
      {:override_text_color (color 250 250 250)}})

    ;;---------- main-wire ------------
    (defloop main-wire
      (Setup
      (initialize-character)
      (initialize-coin)
      (initialize-game-elements)
      (initialize-spiked-canonballs))

      (coin-gravity-logic .coiny-1 .coinx-1 .coin-velocity-1 .coin-position-1)
      (coin-gravity-logic .coiny-2 .coinx-2 .coin-velocity-2 .coin-position-2)

      (run-logic)
      (gravity-logic)
      (scoring)

      (Step idle-animation)
      (Step walking-animation)
      (Step coin-animation)

      (Step random-coin-1)
      (Step random-coin-2)

      (GFX.MainWindow
      :Title "MainWindow" :Width 1920 :Height 1080
      :Contents
      (-> (Setup
            (GFX.DrawQueue) >= .ui-draw-queue
            (GFX.UIPass .ui-draw-queue) >> .render-steps)
          .ui-draw-queue (GFX.ClearQueue)

          (UI
            .ui-draw-queue
            (->
            (UI.Area :Position .character-position
                      :Anchor Anchor.Top
                      :Contents (->
                                .character-state
                                (Match [0 (-> .character-direction
                                              (Match [0 (-> .idle-left-image-array (Take .idle-image-index) (UI.Image :Scale (float2 0.2)))
                                                      1 (-> .idle-right-image-array (Take .idle-image-index) (UI.Image :Scale (float2 0.2)))]
                                                      :Passthrough false))
                                        1 (-> .walking-left-image-array (Take .walking-image-index) (UI.Image :Scale (float2 0.2)))
                                        2 (-> .walking-right-image-array (Take .walking-image-index) (UI.Image :Scale (float2 0.2)))
                                        3 (->  .character-direction
                                                (Match [0 (-> .character-jumping-left (UI.Image :Scale (float2 0.2)))
                                                        1 (-> .character-jumping-right (UI.Image :Scale (float2 0.2)))]
                                                      :Passthrough false))]
                                        :Passthrough false)))

            (UI.Area :Position .coin-position-1
                      :Anchor Anchor.Top
                      :Contents (->
                                .coin-image-array (Take .coin-image-index) (UI.Image :Scale (float2 0.2))))

            (UI.Area :Position .coin-position-2
                      :Anchor Anchor.Top
                      :Contents (->
                                .coin-image-array (Take .coin-image-index) (UI.Image :Scale (float2 0.2))))
            
            (UI.Area :Position (float2 0 0)
                      :Anchor Anchor.Top
                      :Contents (->
                                .spikeball-1 (UI.Image :Scale (float2 0.15))))

            (UI.Area :Position (float2 -40 20)
                      :Anchor Anchor.TopRight
                      :Contents (->
                                style (UI.Style)
                                .score (ToString) (UI.Label)))))

          (button-inputs)

          (GFX.Render :Steps .render-steps))))


    (defmesh main)
    (schedule main main-wire)
    (run main (/ 1.0 60))
    ```

## Step 6.2

Similar to what we did with our coin, it's time to animate our spiked cannonball.

Follow these steps to create an animation once more:

1. Create an `Image` array.

2. Create the necessary variables - an index, max index, and animation speed variable.

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
    ;; ------------ Initialize Spiked CanonBalls ---------------
    (defshards initialize-spiked-canonballs []
      (LoadTexture "GlodImages/SpikeBall/SpikeBall1.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall2.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall3.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall4.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall5.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall6.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall7.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall8.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall9.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall10.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall11.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall12.png") >> .spikeball-array

      (Count .spikeball-array) (Math.Subtract 1) >= .spikeball-array-index-max
      0 >= .spikeball-index
      0.06 >= .spikeball-animation-speed
    ```

 Create an animation loop that uses the variables we created. This loop should add one to our index every iteration. It should also have a conditional statement that ensures that our index does not go above the `index-max`.
 
 Lastly, the loop should loop every x seconds, with x being the animation speed.

    ```{.clojure .annotate linenums="1"}
    ;;------------- Spiked CanonBall Animation -------------
    (defloop spiked-canonball-animation
      .spikeball-index (Math.Add 1)
      > .spikeball-index
      (When :Predicate (IsMore .spikeball-array-index-max)
            :Action (-> 0 > .spikeball-index))

      (Pause .spikeball-animation-speed))
    ```

As with all loops, remember to `Step` it in `main-wire`.

    ```{.clojure .annotate linenums="1"}
    (Step spiked-canonball-animation) ;; (1)

    ```

Remember to update our spiked cannonball's `UI.Area` to display the correct image in the image array, as specified by `.spikeball-index`.

    ```{.clojure .annotate linenums="1"}
    ;; ------------ Character Run Logic ----------------
    ;; -------- Spiked CanonBall UI.Area ----------
    (UI.Area :Position (float2 0 0)
                      :Anchor Anchor.Top
                      :Contents (->
                                .spikeball-array (Take .spikeball-index) (UI.Image :Scale (float2 0.15))))
    ```

=== "Full Code So Far"
    
    ```{.clojure .annotate linenums="1"}
    (defshards LoadTexture [name]
      (LoadImage name)
      (GFX.Texture))

    (defshards initialize-character []
      (LoadTexture "GlodImages/Character1_Jumping_Left.png") = .character-jumping-left
      (LoadTexture "GlodImages/Character1_Jumping_Right.png") = .character-jumping-right

      0 >= .character-state
      0 >= .character-direction
      true >= .can-jump

      0.0 >= .X
      620.0 >= .Y
      (float2 .X .Y) >= .character-position
      0.0 >= .character-x-velocity
      0.0 >= .character-y-velocity
      0.0 >= .character-y-acceleration

      ;; ---------- Character Idle Array (Facing Left) ----------
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_1.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_2.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_3.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_4.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_5.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_6.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_7.png") >> .idle-left-image-array

      ;; ---------- Character Idle Array (Facing Right) ----------------
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_1.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_2.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_3.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_4.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_5.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_6.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_7.png") >> .idle-right-image-array

      0 >= .idle-image-index
      (Count .idle-left-image-array) = .idle-image-index-max
      0.08 = .idle-animation-speed

      ;; -------------- Walking Array (Facing Left) -----------------
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_1.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_2.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_3.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_4.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_5.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_6.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_7.png") >> .walking-left-image-array

      ;; ----------- Walking Array (Facing Right) ---------------
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_1.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_2.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_3.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_4.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_5.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_6.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_7.png") >> .walking-right-image-array

      (Count .walking-left-image-array) = .walking-image-index-max
      0 >= .walking-image-index
      0.08 = .walking-animation-speed) ;; Reduce number to increase animation speed

    ;; --------- Idle Animation Loop ---------
    (defloop idle-animation
      .idle-image-index (Math.Add 1)
      > .idle-image-index
      (When :Predicate (IsMoreEqual .idle-image-index-max)
            :Action (-> 0 > .idle-image-index))
      (Pause .idle-animation-speed))

    ;; -------- Walking Animation Loop --------
    (defloop walking-animation
      .walking-image-index (Math.Add 1)
      > .walking-image-index
      (When :Predicate (IsMoreEqual .walking-image-index-max)
            :Action (-> 0 > .walking-image-index))
      (Pause .walking-animation-speed))

    ;; ---------- character-boundary ------------

    (defshards clamp [var min max]
      var (Max min) (Min max) > var)

    ;; ------------ Character Run Logic ----------------
    (defshards run-logic []
      .X (Math.Add .character-x-velocity)
      > .X

      (float2 .X .Y) > .character-position

      (clamp .X -600.0 600.0))

    ;; ------------ Character gravity-logic ---------------
    (defshards gravity-logic []
      .Y (Math.Add .character-y-velocity)
      > .Y

      .character-y-velocity (Math.Add .character-y-acceleration)
      > .character-y-velocity

      (float2 .X .Y) > .character-position

      (clamp .Y -620.0 620.0)
      .Y
      (When :Predicate (IsMoreEqual 620.0)
            :Action (->
                    0.0 > .character-y-velocity
                    0.0 > .character-y-acceleration
                    true > .can-jump
                    .character-state
                    (When :Predicate (Is 3)
                          :Action (->
                                    0 > .character-state)))))

    ;; ------- Button Inputs ----------
    (defshards button-inputs []
      (Inputs.KeyDown
      :Key "left"
      :Action (->
                (Msg "left")

                .character-state
                (When :Predicate (Is 0)
                      :Action (-> 1 > .character-state))

                0 > .character-direction
                -5.0 > .character-x-velocity))

      (Inputs.KeyDown
      :Key "right"
      :Action (->
                (Msg "right")

                .character-state
                (When :Predicate (Is 0)
                      :Action (-> 2 > .character-state))
                1 > .character-direction
                5.0 > .character-x-velocity))

      (Inputs.KeyDown
      :Key "up"
      :Action (->
                (Msg "up")
                3 > .character-state
                .can-jump
                (When :Predicate (Is true)
                      :Action (->
                              -20.0 > .character-y-velocity
                              1.0 >  .character-y-acceleration
                              false >= .can-jump))))

      (Inputs.KeyUp
      :Key "left"
      :Action (->
                0 > .character-state
                0.0 > .character-x-velocity))

      (Inputs.KeyUp
      :Key "right"
      :Action (->
                0 > .character-state
                0.0 > .character-x-velocity)))

    ;; -------------- Initialize Coin ----------
    (defshards initialize-coin []
      (LoadTexture "GlodImages/Coin/Coin_1.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_2.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_3.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_4.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_5.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_6.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_7.png") >> .coin-image-array
      (Count .coin-image-array) = .coin-image-index-max
      0 >= .coin-image-index
      0.1 = .coin-animation-speed

      ;; ----- Coin 1 ------
      0.0 >= .coinx-1
      0.0 >= .coiny-1
      (float2 .coinx-1 .coiny-1) >= .coin-position-1
      0.0 >= .coin-velocity-1
      0.5 >= .coin-acceleration

      ;; ----- Coin 2 ----
      0.0 >= .coinx-2
      0.0 >= .coiny-2
      (float2 .coinx-2 .coiny-2) >= .coin-position-2
      0.0 >= .coin-velocity-2)

    ;; -------------- Coin Animation ------------------
    (defloop coin-animation
      .coin-image-index (Math.Add 1)
      > .coin-image-index
      (When :Predicate (IsMoreEqual .coin-image-index-max)
            :Action (-> 0 > .coin-image-index))

      (Pause .coin-animation-speed))

    ;; ------------- Coin Gravity ------------------
    (defshards coin-gravity-logic [coiny coinx coin-velocity coin-position]

      coiny (Math.Add coin-velocity)
      > coiny

      coin-velocity (Math.Add .coin-acceleration)
      > coin-velocity

      (float2 coinx coiny) > coin-position)

    ;; ------------- Random Coin ------------------
    (defshards random-coin [coinx coiny coin-velocity coin-position pause-length]
      coinx
      (RandomFloat :Max 1200.0)
      > coinx
      (Math.Subtract 600.0)
      > coinx

      0.0 > coiny
      0.0 > coin-velocity
      (float2 coinx coiny) > coin-position
      (Pause pause-length))

    (defloop random-coin-1
      (random-coin .coinx-1 .coiny-1 .coin-velocity-1 .coin-position-1 1.5))

    (defloop random-coin-2
      (random-coin .coinx-2 .coiny-2 .coin-velocity-2 .coin-position-2 2.5))

    ;; ------------ Initialize Spiked CanonBalls ---------------
    (defshards initialize-spiked-canonballs []
      (LoadTexture "GlodImages/SpikeBall/SpikeBall1.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall2.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall3.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall4.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall5.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall6.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall7.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall8.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall9.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall10.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall11.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall12.png") >> .spikeball-array

      (Count .spikeball-array) = .spikeball-array-index-max
      0 >= .spikeball-index
      0.06 = .spikeball-animation-speed)

    ;;------------- Spiked CanonBall Animation -------------
    (defloop spiked-canonball-animation
      .spikeball-index (Math.Add 1)
      > .spikeball-index
      (When :Predicate (IsMoreEqual .spikeball-array-index-max)
            :Action (-> 0 > .spikeball-index))

      (Pause .spikeball-animation-speed))

    ;; --------- Game Elements ------------
    (defshards initialize-game-elements []
      0 >= .score
      false >= .scored

      .X (Math.Add 50.0)
      >= .scoringUpper-x-limit
      .X (Math.Subtract 50.0)
      >= .scoringLower-x-limit

      .Y (Math.Add 10.0)
      >= .scoringUpper-y-limit
      .Y (Math.Subtract 10.0)
      >= .scoringLower-y-limit)

    ;; --------- Scoring ----------
    (defshards score-collision [coinx coiny]
      coinx
      (When :Predicate (->
                        (IsLess .scoringUpper-x-limit)
                        (And)
                        coinx (IsMore .scoringLower-x-limit)
                        (And)
                        coiny (IsLess .scoringUpper-y-limit)
                        (And)
                        coiny (IsMore .scoringLower-y-limit)
                        (And)
                        .scored (Is false))
            :Action (->
                    true > .scored
                    (Log "Score: "))))

    (defshards scoring []
      .X (Math.Add 50.0)
      > .scoringUpper-x-limit
      .X (Math.Subtract 50.0)
      > .scoringLower-x-limit

      .Y (Math.Add 10.0)
      > .scoringUpper-y-limit
      .Y (Math.Subtract 10.0)
      > .scoringLower-y-limit

      (score-collision .coinx-1 .coiny-1)
      (score-collision .coinx-2 .coiny-2)

      .scored
      (When :Predicate (Is true)
            :Action (->
                    .score (Math.Add 1)
                    > .score
                    false > .scored)))



    ;; ------ UI Style --------
    (def style
      {:override_text_style "MyStyle"
      :text_styles
      [{:name "MyStyle"
        :size (float 46)
        :family "Monospace"}]
      :visuals
      {:override_text_color (color 250 250 250)}})

    ;;---------- main-wire ------------
    (defloop main-wire
      (Setup
      (initialize-character)
      (initialize-coin)
      (initialize-game-elements)
      (initialize-spiked-canonballs))

      (coin-gravity-logic .coiny-1 .coinx-1 .coin-velocity-1 .coin-position-1)
      (coin-gravity-logic .coiny-2 .coinx-2 .coin-velocity-2 .coin-position-2)

      (run-logic)
      (gravity-logic)
      (scoring)

      (Step idle-animation)
      (Step walking-animation)
      
      (Step coin-animation)
      (Step random-coin-1)
      (Step random-coin-2)

      (Step spiked-canonball-animation)

      (GFX.MainWindow
      :Title "MainWindow" :Width 1920 :Height 1080
      :Contents
      (-> (Setup
            (GFX.DrawQueue) >= .ui-draw-queue
            (GFX.UIPass .ui-draw-queue) >> .render-steps)
          .ui-draw-queue (GFX.ClearQueue)

          (UI
            .ui-draw-queue
            (->
            (UI.Area :Position .character-position
                      :Anchor Anchor.Top
                      :Contents (->
                                .character-state
                                (Match [0 (-> .character-direction
                                              (Match [0 (-> .idle-left-image-array (Take .idle-image-index) (UI.Image :Scale (float2 0.2)))
                                                      1 (-> .idle-right-image-array (Take .idle-image-index) (UI.Image :Scale (float2 0.2)))]
                                                      :Passthrough false))
                                        1 (-> .walking-left-image-array (Take .walking-image-index) (UI.Image :Scale (float2 0.2)))
                                        2 (-> .walking-right-image-array (Take .walking-image-index) (UI.Image :Scale (float2 0.2)))
                                        3 (->  .character-direction
                                                (Match [0 (-> .character-jumping-left (UI.Image :Scale (float2 0.2)))
                                                        1 (-> .character-jumping-right (UI.Image :Scale (float2 0.2)))]
                                                      :Passthrough false))]
                                        :Passthrough false)))

            (UI.Area :Position .coin-position-1
                      :Anchor Anchor.Top
                      :Contents (->
                                .coin-image-array (Take .coin-image-index) (UI.Image :Scale (float2 0.2))))

            (UI.Area :Position .coin-position-2
                      :Anchor Anchor.Top
                      :Contents (->
                                .coin-image-array (Take .coin-image-index) (UI.Image :Scale (float2 0.2))))

            (UI.Area :Position (float2 0 0)
                      :Anchor Anchor.Top
                      :Contents (->
                                .spikeball-array (Take .spikeball-index) (UI.Image :Scale (float2 0.15))))

            (UI.Area :Position (float2 -40 20)
                      :Anchor Anchor.TopRight
                      :Contents (->
                                style (UI.Style)
                                .score (ToString) (UI.Label)))))

          (button-inputs)

          (GFX.Render :Steps .render-steps))))


    (defmesh main)
    (schedule main main-wire)
    (run main (/ 1.0 60))
    ```

## Step 6.3

Time to make our spiked cannonball fall.

As in the previous chapter, we will:

1. Create variables that dictate the position of our spiked cannonball.

2. Change our `UI.Area`'s `position` parameter to use this variable.

3. Change the variable by using velocity and acceleration.

Initialize the code variables that we will be using to move our spiked cannonball.

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
    ;; ---------- spikball-1 -------------
    1.0 >= .spikeball-velocity-1
    0.0 >= .spikeball-y-1
    0.0 >= .spikeball-x-1
    (float2 .spikeball-x-1 .spikeball-y-1) >= .spikeball-position-1

    0.5 >= .spikeball-acceleration
    ```

Locate the `UI.Area` housing our spiked cannonball image.

Replace the value of the `Position` attribute with the `.spikeball-position-1` variable.

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
    ;; -------- Spiked CanonBall UI.Area ----------
    (UI.Area :Position .spikeball-position-1
            :Anchor Anchor.Top
            :Contents (->
                          LoadTexture .spikeball-array (Take .spikeball-index) (UI.Image :Scale (float2 0.15))))
    ```

To make our image move, we create the gravity logic for it.

Create a shard that accepts arguments to allow for the creating of more spiked cannonballs in the future.

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
    ;; ------------- SpikeBall Gravity Logic -------------
    (defshards spikeball-gravity-logic [spikeball-y spikeball-velocity spikeball-position spikeball-x]
      spikeball-y (Math.Add spikeball-velocity)
      > spikeball-y
      spikeball-velocity (Math.Add .spikeball-acceleration)
      > spikeball-velocity
      (float2 spikeball-x spikeball-y) > spikeball-position)
    ```

Remember to call your shard in the `main-wire` with the appropriate variables passed in.

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
    (spikeball-gravity-logic .spikeball-y-1 .spikeball-velocity-1 .spikeball-position-1 .spikeball-x-1)
    ```

=== "Full Code So Far"
    
    ```{.clojure .annotate linenums="1"}
    (defshards LoadTexture [name]
      (LoadImage name)
      (GFX.Texture))

    (defshards initialize-character []
      (LoadTexture "GlodImages/Character1_Jumping_Left.png") = .character-jumping-left
      (LoadTexture "GlodImages/Character1_Jumping_Right.png") = .character-jumping-right

      0 >= .character-state
      0 >= .character-direction
      true >= .can-jump

      0.0 >= .X
      620.0 >= .Y
      (float2 .X .Y) >= .character-position
      0.0 >= .character-x-velocity
      0.0 >= .character-y-velocity
      0.0 >= .character-y-acceleration

      ;; ---------- Character Idle Array (Facing Left) ----------
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_1.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_2.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_3.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_4.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_5.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_6.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_7.png") >> .idle-left-image-array

      ;; ---------- Character Idle Array (Facing Right) ----------------
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_1.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_2.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_3.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_4.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_5.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_6.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_7.png") >> .idle-right-image-array

      0 >= .idle-image-index
      (Count .idle-left-image-array) = .idle-image-index-max
      0.08 = .idle-animation-speed

      ;; -------------- Walking Array (Facing Left) -----------------
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_1.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_2.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_3.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_4.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_5.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_6.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_7.png") >> .walking-left-image-array

      ;; ----------- Walking Array (Facing Right) ---------------
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_1.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_2.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_3.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_4.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_5.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_6.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_7.png") >> .walking-right-image-array

      (Count .walking-left-image-array) = .walking-image-index-max
      0 >= .walking-image-index
      0.08 = .walking-animation-speed) ;; Reduce number to increase animation speed

    ;; --------- Idle Animation Loop ---------
    (defloop idle-animation
      .idle-image-index (Math.Add 1)
      > .idle-image-index
      (When :Predicate (IsMoreEqual .idle-image-index-max)
            :Action (-> 0 > .idle-image-index))
      (Pause .idle-animation-speed))

    ;; -------- Walking Animation Loop --------
    (defloop walking-animation
      .walking-image-index (Math.Add 1)
      > .walking-image-index
      (When :Predicate (IsMoreEqual .walking-image-index-max)
            :Action (-> 0 > .walking-image-index))
      (Pause .walking-animation-speed))

    ;; ---------- character-boundary ------------

    (defshards clamp [var min max]
      var (Max min) (Min max) > var)

    ;; ------------ Character Run Logic ----------------
    (defshards run-logic []
      .X (Math.Add .character-x-velocity)
      > .X

      (float2 .X .Y) > .character-position

      (clamp .X -600.0 600.0))

    ;; ------------ Character gravity-logic ---------------
    (defshards gravity-logic []
      .Y (Math.Add .character-y-velocity)
      > .Y

      .character-y-velocity (Math.Add .character-y-acceleration)
      > .character-y-velocity

      (float2 .X .Y) > .character-position

      (clamp .Y -620.0 620.0)
      .Y
      (When :Predicate (IsMoreEqual 620.0)
            :Action (->
                    0.0 > .character-y-velocity
                    0.0 > .character-y-acceleration
                    true > .can-jump
                    .character-state
                    (When :Predicate (Is 3)
                          :Action (->
                                    0 > .character-state)))))

    ;; ------- Button Inputs ----------
    (defshards button-inputs []
      (Inputs.KeyDown
      :Key "left"
      :Action (->
                (Msg "left")

                .character-state
                (When :Predicate (Is 0)
                      :Action (-> 1 > .character-state))

                0 > .character-direction
                -5.0 > .character-x-velocity))

      (Inputs.KeyDown
      :Key "right"
      :Action (->
                (Msg "right")

                .character-state
                (When :Predicate (Is 0)
                      :Action (-> 2 > .character-state))
                1 > .character-direction
                5.0 > .character-x-velocity))

      (Inputs.KeyDown
      :Key "up"
      :Action (->
                (Msg "up")
                3 > .character-state
                .can-jump
                (When :Predicate (Is true)
                      :Action (->
                              -20.0 > .character-y-velocity
                              1.0 >  .character-y-acceleration
                              false >= .can-jump))))

      (Inputs.KeyUp
      :Key "left"
      :Action (->
                0 > .character-state
                0.0 > .character-x-velocity))

      (Inputs.KeyUp
      :Key "right"
      :Action (->
                0 > .character-state
                0.0 > .character-x-velocity)))

    ;; -------------- Initialize Coin ----------
    (defshards initialize-coin []
      (LoadTexture "GlodImages/Coin/Coin_1.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_2.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_3.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_4.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_5.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_6.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_7.png") >> .coin-image-array
      (Count .coin-image-array) = .coin-image-index-max
      0 >= .coin-image-index
      0.1 = .coin-animation-speed

      ;; ----- Coin 1 ------
      0.0 >= .coinx-1
      0.0 >= .coiny-1
      (float2 .coinx-1 .coiny-1) >= .coin-position-1
      0.0 >= .coin-velocity-1
      0.5 >= .coin-acceleration

      ;; ----- Coin 2 ----
      0.0 >= .coinx-2
      0.0 >= .coiny-2
      (float2 .coinx-2 .coiny-2) >= .coin-position-2
      0.0 >= .coin-velocity-2)

    ;; -------------- Coin Animation ------------------
    (defloop coin-animation
      .coin-image-index (Math.Add 1)
      > .coin-image-index
      (When :Predicate (IsMoreEqual .coin-image-index-max)
            :Action (-> 0 > .coin-image-index))

      (Pause .coin-animation-speed))

    ;; ------------- Coin Gravity ------------------
    (defshards coin-gravity-logic [coiny coinx coin-velocity coin-position]

      coiny (Math.Add coin-velocity)
      > coiny

      coin-velocity (Math.Add .coin-acceleration)
      > coin-velocity

      (float2 coinx coiny) > coin-position)

    ;; ------------- Random Coin ------------------
    (defshards random-coin [coinx coiny coin-velocity coin-position pause-length]
      coinx
      (RandomFloat :Max 1200.0)
      > coinx
      (Math.Subtract 600.0)
      > coinx

      0.0 > coiny
      0.0 > coin-velocity
      (float2 coinx coiny) > coin-position
      (Pause pause-length))

    (defloop random-coin-1
      (random-coin .coinx-1 .coiny-1 .coin-velocity-1 .coin-position-1 1.5))

    (defloop random-coin-2
      (random-coin .coinx-2 .coiny-2 .coin-velocity-2 .coin-position-2 2.5))

    ;; ------------ Initialize Spiked CanonBalls ---------------
    (defshards initialize-spiked-canonballs []
      (LoadTexture "GlodImages/SpikeBall/SpikeBall1.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall2.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall3.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall4.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall5.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall6.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall7.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall8.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall9.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall10.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall11.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall12.png") >> .spikeball-array

      (Count .spikeball-array) = .spikeball-array-index-max
      0 >= .spikeball-index
      0.06 = .spikeball-animation-speed

      ;; ---------- spikball-1 -------------
      1.0 >= .spikeball-velocity-1
      0.0 >= .spikeball-y-1
      0.0 >= .spikeball-x-1
      (float2 .spikeball-x-1 .spikeball-y-1) >= .spikeball-position-1

      0.5 >= .spikeball-acceleration)

    ;;------------- Spiked CanonBall Animation -------------
    (defloop spiked-canonball-animation
      .spikeball-index (Math.Add 1)
      > .spikeball-index
      (When :Predicate (IsMoreEqual .spikeball-array-index-max)
            :Action (-> 0 > .spikeball-index))

      (Pause .spikeball-animation-speed))

    ;; ------------- SpikeBall_Gravity_Logic -------------
    (defshards spikeball-gravity-logic [spikeball-y spikeball-velocity spikeball-position spikeball-x]
      spikeball-y (Math.Add spikeball-velocity)
      > spikeball-y
      spikeball-velocity (Math.Add .spikeball-acceleration)
      > spikeball-velocity
      (float2 spikeball-x spikeball-y) > spikeball-position)

    ;; --------- Game Elements ------------
    (defshards initialize-game-elements []
      0 >= .score
      false >= .scored

      .X (Math.Add 50.0)
      >= .scoringUpper-x-limit
      .X (Math.Subtract 50.0)
      >= .scoringLower-x-limit

      .Y (Math.Add 10.0)
      >= .scoringUpper-y-limit
      .Y (Math.Subtract 10.0)
      >= .scoringLower-y-limit)

    ;; --------- Scoring ----------
    (defshards score-collision [coinx coiny]
      coinx
      (When :Predicate (->
                        (IsLess .scoringUpper-x-limit)
                        (And)
                        coinx (IsMore .scoringLower-x-limit)
                        (And)
                        coiny (IsLess .scoringUpper-y-limit)
                        (And)
                        coiny (IsMore .scoringLower-y-limit)
                        (And)
                        .scored (Is false))
            :Action (->
                    true > .scored
                    (Log "Score: "))))

    (defshards scoring []
      .X (Math.Add 50.0)
      > .scoringUpper-x-limit
      .X (Math.Subtract 50.0)
      > .scoringLower-x-limit

      .Y (Math.Add 10.0)
      > .scoringUpper-y-limit
      .Y (Math.Subtract 10.0)
      > .scoringLower-y-limit

      (score-collision .coinx-1 .coiny-1)
      (score-collision .coinx-2 .coiny-2)

      .scored
      (When :Predicate (Is true)
            :Action (->
                    .score (Math.Add 1)
                    > .score
                    false > .scored)))



    ;; ------ UI Style --------
    (def style
      {:override_text_style "MyStyle"
      :text_styles
      [{:name "MyStyle"
        :size (float 46)
        :family "Monospace"}]
      :visuals
      {:override_text_color (color 250 250 250)}})

    ;;---------- main-wire ------------
    (defloop main-wire
      (Setup
      (initialize-character)
      (initialize-coin)
      (initialize-game-elements)
      (initialize-spiked-canonballs))

      (coin-gravity-logic .coiny-1 .coinx-1 .coin-velocity-1 .coin-position-1)
      (coin-gravity-logic .coiny-2 .coinx-2 .coin-velocity-2 .coin-position-2)

      (run-logic)
      (gravity-logic)
      (scoring)

      (Step idle-animation)
      (Step walking-animation)

      (Step coin-animation)
      (Step random-coin-1)
      (Step random-coin-2)

      (Step spiked-canonball-animation)
      (spikeball-gravity-logic .spikeball-y-1 .spikeball-velocity-1 .spikeball-position-1 .spikeball-x-1)

      (GFX.MainWindow
      :Title "MainWindow" :Width 1920 :Height 1080
      :Contents
      (-> (Setup
            (GFX.DrawQueue) >= .ui-draw-queue
            (GFX.UIPass .ui-draw-queue) >> .render-steps)
          .ui-draw-queue (GFX.ClearQueue)

          (UI
            .ui-draw-queue
            (->
            (UI.Area :Position .character-position
                      :Anchor Anchor.Top
                      :Contents (->
                                .character-state
                                (Match [0 (-> .character-direction
                                              (Match [0 (-> .idle-left-image-array (Take .idle-image-index) (UI.Image :Scale (float2 0.2)))
                                                      1 (-> .idle-right-image-array (Take .idle-image-index) (UI.Image :Scale (float2 0.2)))]
                                                      :Passthrough false))
                                        1 (-> .walking-left-image-array (Take .walking-image-index) (UI.Image :Scale (float2 0.2)))
                                        2 (-> .walking-right-image-array (Take .walking-image-index) (UI.Image :Scale (float2 0.2)))
                                        3 (->  .character-direction
                                                (Match [0 (-> .character-jumping-left (UI.Image :Scale (float2 0.2)))
                                                        1 (-> .character-jumping-right (UI.Image :Scale (float2 0.2)))]
                                                      :Passthrough false))]
                                        :Passthrough false)))

            (UI.Area :Position .coin-position-1
                      :Anchor Anchor.Top
                      :Contents (->
                                .coin-image-array (Take .coin-image-index) (UI.Image :Scale (float2 0.2))))

            (UI.Area :Position .coin-position-2
                      :Anchor Anchor.Top
                      :Contents (->
                                .coin-image-array (Take .coin-image-index) (UI.Image :Scale (float2 0.2))))

            (UI.Area :Position .spikeball-position-1
                      :Anchor Anchor.Top
                      :Contents (->
                                .spikeball-array (Take .spikeball-index) (UI.Image :Scale (float2 0.15))))

            (UI.Area :Position (float2 -40 20)
                      :Anchor Anchor.TopRight
                      :Contents (->
                                style (UI.Style)
                                .score (ToString) (UI.Label)))))

          (button-inputs)

          (GFX.Render :Steps .render-steps))))


    (defmesh main)
    (schedule main main-wire)
    (run main (/ 1.0 60))
    ```

Try running your code now to see if it falls!

## Step 6.4

To make sure that our spiked cannonball loops and randomly spawns in different locations, we will implement the same spawning logic used previously.

Create our `randomise-spikeball` logic. Similar to our coin, we reset the spiked cannonball's y value back to zero, so that it appears at the top of the screen again.

This time however, we will have different randomized x values and have this happen every `pausefloat` number of seconds.

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
    ;; ------------- Randomise Spikeball ----------------
    (defshards randomise-spikeball [spikeball-x spikeball-y spikeball-velocity spikeball-position pausefloat]
      spikeball-x
      (RandomFloat :Max 1200.0)
      > spikeball-x
      (Math.Subtract 600.0)
      > spikeball-x

      0.0 > spikeball-y
      0.0 > spikeball-velocity
      (float2 spikeball-x spikeball-y) > spikeball-position
      
      (Pause pausefloat))
    ```

We then plug this shard into a loop and fill in the appropriate variables.

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
    (defloop spikeball-1
      (randomise-spikeball .spikeball-x-1 .spikeball-y-1 .spikeball-velocity-1 .spikeball-position-1 2))
    ```

Call your `spikeball-1` loop in the `main-wire`.

=== "Code Added"

    ```{.clojure .annotate linenums="1"}
    ;; ------- Button Inputs ----------
    (Step spikeball-1)
    ```

=== "Full Code So Far"
    
    ```{.clojure .annotate linenums="1"}
    (defshards LoadTexture [name]
      (LoadImage name)
      (GFX.Texture))

    (defshards initialize-character []
      (LoadTexture "GlodImages/Character1_Jumping_Left.png") = .character-jumping-left
      (LoadTexture "GlodImages/Character1_Jumping_Right.png") = .character-jumping-right

      0 >= .character-state
      0 >= .character-direction
      true >= .can-jump

      0.0 >= .X
      620.0 >= .Y
      (float2 .X .Y) >= .character-position
      0.0 >= .character-x-velocity
      0.0 >= .character-y-velocity
      0.0 >= .character-y-acceleration

      ;; ---------- Character Idle Array (Facing Left) ----------
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_1.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_2.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_3.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_4.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_5.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_6.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_7.png") >> .idle-left-image-array

      ;; ---------- Character Idle Array (Facing Right) ----------------
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_1.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_2.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_3.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_4.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_5.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_6.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_7.png") >> .idle-right-image-array

      0 >= .idle-image-index
      (Count .idle-left-image-array) = .idle-image-index-max
      0.08 = .idle-animation-speed

      ;; -------------- Walking Array (Facing Left) -----------------
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_1.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_2.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_3.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_4.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_5.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_6.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_7.png") >> .walking-left-image-array

      ;; ----------- Walking Array (Facing Right) ---------------
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_1.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_2.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_3.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_4.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_5.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_6.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_7.png") >> .walking-right-image-array

      (Count .walking-left-image-array) = .walking-image-index-max
      0 >= .walking-image-index
      0.08 = .walking-animation-speed) ;; Reduce number to increase animation speed

    ;; --------- Idle Animation Loop ---------
    (defloop idle-animation
      .idle-image-index (Math.Add 1)
      > .idle-image-index
      (When :Predicate (IsMoreEqual .idle-image-index-max)
            :Action (-> 0 > .idle-image-index))
      (Pause .idle-animation-speed))

    ;; -------- Walking Animation Loop --------
    (defloop walking-animation
      .walking-image-index (Math.Add 1)
      > .walking-image-index
      (When :Predicate (IsMoreEqual .walking-image-index-max)
            :Action (-> 0 > .walking-image-index))
      (Pause .walking-animation-speed))

    ;; ---------- character-boundary ------------

    (defshards clamp [var min max]
      var (Max min) (Min max) > var)

    ;; ------------ Character Run Logic ----------------
    (defshards run-logic []
      .X (Math.Add .character-x-velocity)
      > .X

      (float2 .X .Y) > .character-position

      (clamp .X -600.0 600.0))

    ;; ------------ Character gravity-logic ---------------
    (defshards gravity-logic []
      .Y (Math.Add .character-y-velocity)
      > .Y

      .character-y-velocity (Math.Add .character-y-acceleration)
      > .character-y-velocity

      (float2 .X .Y) > .character-position

      (clamp .Y -620.0 620.0)
      .Y
      (When :Predicate (IsMoreEqual 620.0)
            :Action (->
                    0.0 > .character-y-velocity
                    0.0 > .character-y-acceleration
                    true > .can-jump
                    .character-state
                    (When :Predicate (Is 3)
                          :Action (->
                                    0 > .character-state)))))

    ;; ------- Button Inputs ----------
    (defshards button-inputs []
      (Inputs.KeyDown
      :Key "left"
      :Action (->
                (Msg "left")

                .character-state
                (When :Predicate (Is 0)
                      :Action (-> 1 > .character-state))

                0 > .character-direction
                -5.0 > .character-x-velocity))

      (Inputs.KeyDown
      :Key "right"
      :Action (->
                (Msg "right")

                .character-state
                (When :Predicate (Is 0)
                      :Action (-> 2 > .character-state))
                1 > .character-direction
                5.0 > .character-x-velocity))

      (Inputs.KeyDown
      :Key "up"
      :Action (->
                (Msg "up")
                3 > .character-state
                .can-jump
                (When :Predicate (Is true)
                      :Action (->
                              -20.0 > .character-y-velocity
                              1.0 >  .character-y-acceleration
                              false >= .can-jump))))

      (Inputs.KeyUp
      :Key "left"
      :Action (->
                0 > .character-state
                0.0 > .character-x-velocity))

      (Inputs.KeyUp
      :Key "right"
      :Action (->
                0 > .character-state
                0.0 > .character-x-velocity)))

    ;; -------------- Initialize Coin ----------
    (defshards initialize-coin []
      (LoadTexture "GlodImages/Coin/Coin_1.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_2.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_3.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_4.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_5.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_6.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_7.png") >> .coin-image-array
      (Count .coin-image-array) = .coin-image-index-max
      0 >= .coin-image-index
      0.1 = .coin-animation-speed

      ;; ----- Coin 1 ------
      0.0 >= .coinx-1
      0.0 >= .coiny-1
      (float2 .coinx-1 .coiny-1) >= .coin-position-1
      0.0 >= .coin-velocity-1
      0.5 >= .coin-acceleration

      ;; ----- Coin 2 ----
      0.0 >= .coinx-2
      0.0 >= .coiny-2
      (float2 .coinx-2 .coiny-2) >= .coin-position-2
      0.0 >= .coin-velocity-2)

    ;; -------------- Coin Animation ------------------
    (defloop coin-animation
      .coin-image-index (Math.Add 1)
      > .coin-image-index
      (When :Predicate (IsMoreEqual .coin-image-index-max)
            :Action (-> 0 > .coin-image-index))

      (Pause .coin-animation-speed))

    ;; ------------- Coin Gravity ------------------
    (defshards coin-gravity-logic [coiny coinx coin-velocity coin-position]

      coiny (Math.Add coin-velocity)
      > coiny

      coin-velocity (Math.Add .coin-acceleration)
      > coin-velocity

      (float2 coinx coiny) > coin-position)

    ;; ------------- Random Coin ------------------
    (defshards random-coin [coinx coiny coin-velocity coin-position pause-length]
      coinx
      (RandomFloat :Max 1200.0)
      > coinx
      (Math.Subtract 600.0)
      > coinx

      0.0 > coiny
      0.0 > coin-velocity
      (float2 coinx coiny) > coin-position
      (Pause pause-length))

    (defloop random-coin-1
      (random-coin .coinx-1 .coiny-1 .coin-velocity-1 .coin-position-1 1.5))

    (defloop random-coin-2
      (random-coin .coinx-2 .coiny-2 .coin-velocity-2 .coin-position-2 2.5))

    ;; ------------ Initialize Spiked CanonBalls ---------------
    (defshards initialize-spiked-canonballs []
      (LoadTexture "GlodImages/SpikeBall/SpikeBall1.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall2.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall3.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall4.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall5.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall6.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall7.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall8.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall9.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall10.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall11.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall12.png") >> .spikeball-array

      (Count .spikeball-array) = .spikeball-array-index-max
      0 >= .spikeball-index
      0.06 = .spikeball-animation-speed

      ;; ---------- spikball-1 -------------
      1.0 >= .spikeball-velocity-1
      0.0 >= .spikeball-y-1
      0.0 >= .spikeball-x-1
      (float2 .spikeball-x-1 .spikeball-y-1) >= .spikeball-position-1

      0.5 >= .spikeball-acceleration)

    ;;------------- Spiked CanonBall Animation -------------
    (defloop spiked-canonball-animation
      .spikeball-index (Math.Add 1)
      > .spikeball-index
      (When :Predicate (IsMoreEqual .spikeball-array-index-max)
            :Action (-> 0 > .spikeball-index))

      (Pause .spikeball-animation-speed))

    ;; ------------- SpikeBall_Gravity_Logic -------------
    (defshards spikeball-gravity-logic [spikeball-y spikeball-velocity spikeball-position spikeball-x]
      spikeball-y (Math.Add spikeball-velocity)
      > spikeball-y
      spikeball-velocity (Math.Add .spikeball-acceleration)
      > spikeball-velocity
      (float2 spikeball-x spikeball-y) > spikeball-position)

    ;; ------------- Randomise Spikeball ----------------
    (defshards randomise-spikeball [spikeball-x spikeball-y spikeball-velocity spikeball-position pausefloat]
      spikeball-x
      (RandomFloat :Max 1200.0)
      > spikeball-x
      (Math.Subtract 600.0)
      > spikeball-x

      0.0 > spikeball-y
      0.0 > spikeball-velocity
      (float2 spikeball-x spikeball-y) > spikeball-position

      (Pause pausefloat))

    (defloop spikeball-1
      (randomise-spikeball .spikeball-x-1 .spikeball-y-1 .spikeball-velocity-1 .spikeball-position-1 2))

    ;; --------- Game Elements ------------
    (defshards initialize-game-elements []
      0 >= .score
      false >= .scored

      .X (Math.Add 50.0)
      >= .scoringUpper-x-limit
      .X (Math.Subtract 50.0)
      >= .scoringLower-x-limit

      .Y (Math.Add 10.0)
      >= .scoringUpper-y-limit
      .Y (Math.Subtract 10.0)
      >= .scoringLower-y-limit)

    ;; --------- Scoring ----------
    (defshards score-collision [coinx coiny]
      coinx
      (When :Predicate (->
                        (IsLess .scoringUpper-x-limit)
                        (And)
                        coinx (IsMore .scoringLower-x-limit)
                        (And)
                        coiny (IsLess .scoringUpper-y-limit)
                        (And)
                        coiny (IsMore .scoringLower-y-limit)
                        (And)
                        .scored (Is false))
            :Action (->
                    true > .scored
                    (Log "Score: "))))

    (defshards scoring []
      .X (Math.Add 50.0)
      > .scoringUpper-x-limit
      .X (Math.Subtract 50.0)
      > .scoringLower-x-limit

      .Y (Math.Add 10.0)
      > .scoringUpper-y-limit
      .Y (Math.Subtract 10.0)
      > .scoringLower-y-limit

      (score-collision .coinx-1 .coiny-1)
      (score-collision .coinx-2 .coiny-2)

      .scored
      (When :Predicate (Is true)
            :Action (->
                    .score (Math.Add 1)
                    > .score
                    false > .scored)))



    ;; ------ UI Style --------
    (def style
      {:override_text_style "MyStyle"
      :text_styles
      [{:name "MyStyle"
        :size (float 46)
        :family "Monospace"}]
      :visuals
      {:override_text_color (color 250 250 250)}})

    ;;---------- main-wire ------------
    (defloop main-wire
      (Setup
      (initialize-character)
      (initialize-coin)
      (initialize-game-elements)
      (initialize-spiked-canonballs))

      (coin-gravity-logic .coiny-1 .coinx-1 .coin-velocity-1 .coin-position-1)
      (coin-gravity-logic .coiny-2 .coinx-2 .coin-velocity-2 .coin-position-2)

      (run-logic)
      (gravity-logic)
      (scoring)

      (Step idle-animation)
      (Step walking-animation)

      (Step coin-animation)
      (Step random-coin-1)
      (Step random-coin-2)

      (Step spiked-canonball-animation)
      (spikeball-gravity-logic .spikeball-y-1 .spikeball-velocity-1 .spikeball-position-1 .spikeball-x-1)
      (Step  spikeball-1)

      (GFX.MainWindow
      :Title "MainWindow" :Width 1920 :Height 1080
      :Contents
      (-> (Setup
            (GFX.DrawQueue) >= .ui-draw-queue
            (GFX.UIPass .ui-draw-queue) >> .render-steps)
          .ui-draw-queue (GFX.ClearQueue)

          (UI
            .ui-draw-queue
            (->
            (UI.Area :Position .character-position
                      :Anchor Anchor.Top
                      :Contents (->
                                .character-state
                                (Match [0 (-> .character-direction
                                              (Match [0 (-> .idle-left-image-array (Take .idle-image-index) (UI.Image :Scale (float2 0.2)))
                                                      1 (-> .idle-right-image-array (Take .idle-image-index) (UI.Image :Scale (float2 0.2)))]
                                                      :Passthrough false))
                                        1 (-> .walking-left-image-array (Take .walking-image-index) (UI.Image :Scale (float2 0.2)))
                                        2 (-> .walking-right-image-array (Take .walking-image-index) (UI.Image :Scale (float2 0.2)))
                                        3 (->  .character-direction
                                                (Match [0 (-> .character-jumping-left (UI.Image :Scale (float2 0.2)))
                                                        1 (-> .character-jumping-right (UI.Image :Scale (float2 0.2)))]
                                                      :Passthrough false))]
                                        :Passthrough false)))

            (UI.Area :Position .coin-position-1
                      :Anchor Anchor.Top
                      :Contents (->
                                .coin-image-array (Take .coin-image-index) (UI.Image :Scale (float2 0.2))))

            (UI.Area :Position .coin-position-2
                      :Anchor Anchor.Top
                      :Contents (->
                                .coin-image-array (Take .coin-image-index) (UI.Image :Scale (float2 0.2))))

            (UI.Area :Position .spikeball-position-1
                      :Anchor Anchor.Top
                      :Contents (->
                                .spikeball-array (Take .spikeball-index) (UI.Image :Scale (float2 0.15))))

            (UI.Area :Position (float2 -40 20)
                      :Anchor Anchor.TopRight
                      :Contents (->
                                style (UI.Style)
                                .score (ToString) (UI.Label)))))

          (button-inputs)

          (GFX.Render :Steps .render-steps))))


    (defmesh main)
    (schedule main main-wire)
    (run main (/ 1.0 60))
    ```

# Step 6.5

Now that we have our spiked cannonball looping and randomizing, the next step is to have it damage Glod.

Poor Glod. 😢

It will follow the same logic as our coin. However, instead of gaining points when Glod collides with it, points will be lost.

Create the variables that we will be using. They are similar to the collision box we used for scoring.

Add this to the `initialize-game-elements` shard.

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
    ;; ---------- Damage Limits ------------
    .X (Math.Add 50.0)
    >= .damageUpper-x-limit
    .X (Math.Subtract 50.0)
    >= .damageLower-x-limit

    .Y (Math.Add 5.0)
    >= .damageUpper-y-limit
    .Y (Math.Subtract 5.0)
    >= .damageLower-y-limit

    false >= .damaged
    ```

Create the logic to dictate what happens when our spiked cannonball hits Glod. Remember to call this in `main-wire`.

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
    ;; ------------- spikeBall-collision-logic --------------
    (defshards spikeBall-collision-logic [spikeBall-x spikeBall-y]

      spikeBall-x
      (If :Predicate (-> (IsLess .damageUpper-x-limit)
                        (And)
                        spikeBall-x (IsMore .damageLower-x-limit)
                        (And)
                        spikeBall-y (IsLess .damageUpper-y-limit)
                        (And)
                        spikeBall-y (IsMore .damageLower-y-limit))

          :Then (-> .damaged
                    (When :Predicate (Is false)
                          :Action (->
                                  true > .damaged
                                  (Log "damaged: "))))))
    ```

    ```{.clojure .annotate linenums="1"}
    ;; -------------- Damaging --------------
    (defshards damaging []

      .X (Math.Add 120.0)
      > .damageUpper-x-limit
      .X (Math.Subtract 120.0)
      > .damageLower-x-limit

      .Y (Math.Add 15.0)
      > .damageUpper-y-limit
      .Y (Math.Subtract 15.0)
      > .damageLower-y-limit

      (spikeBall-collision-logic .spikeball-x-1 .spikeball-y-1)

      .damaged
      (When :Predicate (Is true)
            :Action (->
                    .score (Math.Subtract 1)
                    > .score
                    false > .damaged)))
    ```

    ```{.clojure .annotate linenums="1"}
    (damaging)
    ```

=== "Full Code So Far"
    
    ```{.clojure .annotate linenums="1"}
    (defshards LoadTexture [name]
      (LoadImage name)
      (GFX.Texture))

    (defshards initialize-character []
      (LoadTexture "GlodImages/Character1_Jumping_Left.png") = .character-jumping-left
      (LoadTexture "GlodImages/Character1_Jumping_Right.png") = .character-jumping-right

      0 >= .character-state
      0 >= .character-direction
      true >= .can-jump

      0.0 >= .X
      620.0 >= .Y
      (float2 .X .Y) >= .character-position
      0.0 >= .character-x-velocity
      0.0 >= .character-y-velocity
      0.0 >= .character-y-acceleration

      ;; ---------- Character Idle Array (Facing Left) ----------
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_1.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_2.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_3.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_4.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_5.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_6.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_7.png") >> .idle-left-image-array

      ;; ---------- Character Idle Array (Facing Right) ----------------
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_1.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_2.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_3.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_4.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_5.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_6.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_7.png") >> .idle-right-image-array

      0 >= .idle-image-index
      (Count .idle-left-image-array) = .idle-image-index-max
      0.08 = .idle-animation-speed

      ;; -------------- Walking Array (Facing Left) -----------------
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_1.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_2.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_3.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_4.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_5.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_6.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_7.png") >> .walking-left-image-array

      ;; ----------- Walking Array (Facing Right) ---------------
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_1.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_2.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_3.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_4.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_5.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_6.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_7.png") >> .walking-right-image-array

      (Count .walking-left-image-array) = .walking-image-index-max
      0 >= .walking-image-index
      0.08 = .walking-animation-speed) ;; Reduce number to increase animation speed

    ;; --------- Idle Animation Loop ---------
    (defloop idle-animation
      .idle-image-index (Math.Add 1)
      > .idle-image-index
      (When :Predicate (IsMoreEqual .idle-image-index-max)
            :Action (-> 0 > .idle-image-index))
      (Pause .idle-animation-speed))

    ;; -------- Walking Animation Loop --------
    (defloop walking-animation
      .walking-image-index (Math.Add 1)
      > .walking-image-index
      (When :Predicate (IsMoreEqual .walking-image-index-max)
            :Action (-> 0 > .walking-image-index))
      (Pause .walking-animation-speed))

    ;; ---------- character-boundary ------------

    (defshards clamp [var min max]
      var (Max min) (Min max) > var)

    ;; ------------ Character Run Logic ----------------
    (defshards run-logic []
      .X (Math.Add .character-x-velocity)
      > .X

      (float2 .X .Y) > .character-position

      (clamp .X -600.0 600.0))

    ;; ------------ Character gravity-logic ---------------
    (defshards gravity-logic []
      .Y (Math.Add .character-y-velocity)
      > .Y

      .character-y-velocity (Math.Add .character-y-acceleration)
      > .character-y-velocity

      (float2 .X .Y) > .character-position

      (clamp .Y -620.0 620.0)
      .Y
      (When :Predicate (IsMoreEqual 620.0)
            :Action (->
                    0.0 > .character-y-velocity
                    0.0 > .character-y-acceleration
                    true > .can-jump
                    .character-state
                    (When :Predicate (Is 3)
                          :Action (->
                                    0 > .character-state)))))

    ;; ------- Button Inputs ----------
    (defshards button-inputs []
      (Inputs.KeyDown
      :Key "left"
      :Action (->
                (Msg "left")

                .character-state
                (When :Predicate (Is 0)
                      :Action (-> 1 > .character-state))

                0 > .character-direction
                -5.0 > .character-x-velocity))

      (Inputs.KeyDown
      :Key "right"
      :Action (->
                (Msg "right")

                .character-state
                (When :Predicate (Is 0)
                      :Action (-> 2 > .character-state))
                1 > .character-direction
                5.0 > .character-x-velocity))

      (Inputs.KeyDown
      :Key "up"
      :Action (->
                (Msg "up")
                3 > .character-state
                .can-jump
                (When :Predicate (Is true)
                      :Action (->
                              -20.0 > .character-y-velocity
                              1.0 >  .character-y-acceleration
                              false >= .can-jump))))

      (Inputs.KeyUp
      :Key "left"
      :Action (->
                0 > .character-state
                0.0 > .character-x-velocity))

      (Inputs.KeyUp
      :Key "right"
      :Action (->
                0 > .character-state
                0.0 > .character-x-velocity)))

    ;; -------------- Initialize Coin ----------
    (defshards initialize-coin []
      (LoadTexture "GlodImages/Coin/Coin_1.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_2.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_3.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_4.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_5.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_6.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_7.png") >> .coin-image-array
      (Count .coin-image-array) = .coin-image-index-max
      0 >= .coin-image-index
      0.1 = .coin-animation-speed

      ;; ----- Coin 1 ------
      0.0 >= .coinx-1
      0.0 >= .coiny-1
      (float2 .coinx-1 .coiny-1) >= .coin-position-1
      0.0 >= .coin-velocity-1
      0.5 >= .coin-acceleration

      ;; ----- Coin 2 ----
      0.0 >= .coinx-2
      0.0 >= .coiny-2
      (float2 .coinx-2 .coiny-2) >= .coin-position-2
      0.0 >= .coin-velocity-2)

    ;; -------------- Coin Animation ------------------
    (defloop coin-animation
      .coin-image-index (Math.Add 1)
      > .coin-image-index
      (When :Predicate (IsMoreEqual .coin-image-index-max)
            :Action (-> 0 > .coin-image-index))

      (Pause .coin-animation-speed))

    ;; ------------- Coin Gravity ------------------
    (defshards coin-gravity-logic [coiny coinx coin-velocity coin-position]

      coiny (Math.Add coin-velocity)
      > coiny

      coin-velocity (Math.Add .coin-acceleration)
      > coin-velocity

      (float2 coinx coiny) > coin-position)

    ;; ------------- Random Coin ------------------
    (defshards random-coin [coinx coiny coin-velocity coin-position pause-length]
      coinx
      (RandomFloat :Max 1200.0)
      > coinx
      (Math.Subtract 600.0)
      > coinx

      0.0 > coiny
      0.0 > coin-velocity
      (float2 coinx coiny) > coin-position
      (Pause pause-length))

    (defloop random-coin-1
      (random-coin .coinx-1 .coiny-1 .coin-velocity-1 .coin-position-1 1.5))

    (defloop random-coin-2
      (random-coin .coinx-2 .coiny-2 .coin-velocity-2 .coin-position-2 2.5))

    ;; ------------ Initialize Spiked CanonBalls ---------------
    (defshards initialize-spiked-canonballs []
      (LoadTexture "GlodImages/SpikeBall/SpikeBall1.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall2.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall3.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall4.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall5.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall6.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall7.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall8.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall9.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall10.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall11.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall12.png") >> .spikeball-array

      (Count .spikeball-array) = .spikeball-array-index-max
      0 >= .spikeball-index
      0.06 = .spikeball-animation-speed

      ;; ---------- spikball-1 -------------
      1.0 >= .spikeball-velocity-1
      0.0 >= .spikeball-y-1
      0.0 >= .spikeball-x-1
      (float2 .spikeball-x-1 .spikeball-y-1) >= .spikeball-position-1

      0.5 >= .spikeball-acceleration)

    ;;------------- Spiked CanonBall Animation -------------
    (defloop spiked-canonball-animation
      .spikeball-index (Math.Add 1)
      > .spikeball-index
      (When :Predicate (IsMoreEqual .spikeball-array-index-max)
            :Action (-> 0 > .spikeball-index))

      (Pause .spikeball-animation-speed))

    ;; ------------- SpikeBall_Gravity_Logic -------------
    (defshards spikeball-gravity-logic [spikeball-y spikeball-velocity spikeball-position spikeball-x]
      spikeball-y (Math.Add spikeball-velocity)
      > spikeball-y
      spikeball-velocity (Math.Add .spikeball-acceleration)
      > spikeball-velocity
      (float2 spikeball-x spikeball-y) > spikeball-position)

    ;; ------------- Randomise Spikeball ----------------
    (defshards randomise-spikeball [spikeball-x spikeball-y spikeball-velocity spikeball-position pausefloat]
      spikeball-x
      (RandomFloat :Max 1200.0)
      > spikeball-x
      (Math.Subtract 600.0)
      > spikeball-x

      0.0 > spikeball-y
      0.0 > spikeball-velocity
      (float2 spikeball-x spikeball-y) > spikeball-position

      (Pause pausefloat))

    (defloop spikeball-1
      (randomise-spikeball .spikeball-x-1 .spikeball-y-1 .spikeball-velocity-1 .spikeball-position-1 2))

    ;; --------- Game Elements ------------
    (defshards initialize-game-elements []
      ;;------------ Scoring Limits ----------
      0 >= .score
      false >= .scored

      .X (Math.Add 50.0)
      >= .scoringUpper-x-limit
      .X (Math.Subtract 50.0)
      >= .scoringLower-x-limit

      .Y (Math.Add 10.0)
      >= .scoringUpper-y-limit
      .Y (Math.Subtract 10.0)
      >= .scoringLower-y-limit

      ;; ---------- Damage Limits ------------
      .X (Math.Add 50.0)
      >= .damageUpper-x-limit
      .X (Math.Subtract 50.0)
      >= .damageLower-x-limit

      .Y (Math.Add 5.0)
      >= .damageUpper-y-limit
      .Y (Math.Subtract 5.0)
      >= .damageLower-y-limit

      false >= .damaged)

    ;; --------- Scoring ----------
    (defshards score-collision [coinx coiny]
      coinx
      (When :Predicate (->
                        (IsLess .scoringUpper-x-limit)
                        (And)
                        coinx (IsMore .scoringLower-x-limit)
                        (And)
                        coiny (IsLess .scoringUpper-y-limit)
                        (And)
                        coiny (IsMore .scoringLower-y-limit)
                        (And)
                        .scored (Is false))
            :Action (->
                    true > .scored
                    (Log "Score: "))))

    (defshards scoring []
      .X (Math.Add 50.0)
      > .scoringUpper-x-limit
      .X (Math.Subtract 50.0)
      > .scoringLower-x-limit

      .Y (Math.Add 10.0)
      > .scoringUpper-y-limit
      .Y (Math.Subtract 10.0)
      > .scoringLower-y-limit

      (score-collision .coinx-1 .coiny-1)
      (score-collision .coinx-2 .coiny-2)

      .scored
      (When :Predicate (Is true)
            :Action (->
                    .score (Math.Add 1)
                    > .score
                    false > .scored)))

    ;; ------------- spikeBall-collision-logic --------------
    (defshards spikeBall-collision-logic [spikeBall-x spikeBall-y]

      spikeBall-x
      (If :Predicate (-> (IsLess .damageUpper-x-limit)
                        (And)
                        spikeBall-x (IsMore .damageLower-x-limit)
                        (And)
                        spikeBall-y (IsLess .damageUpper-y-limit)
                        (And)
                        spikeBall-y (IsMore .damageLower-y-limit))

          :Then (-> .damaged
                    (When :Predicate (Is false)
                          :Action (->
                                  true > .damaged
                                  (Log "damaged: "))))))

    ;; -------------- Damaging --------------
    (defshards damaging []

      .X (Math.Add 120.0)
      > .damageUpper-x-limit
      .X (Math.Subtract 120.0)
      > .damageLower-x-limit

      .Y (Math.Add 15.0)
      > .damageUpper-y-limit
      .Y (Math.Subtract 15.0)
      > .damageLower-y-limit

      (spikeBall-collision-logic .spikeball-x-1 .spikeball-y-1)

      .damaged
      (When :Predicate (Is true)
            :Action (->
                    .score (Math.Subtract 1)
                    > .score
                    false > .damaged)))



    ;; ------ UI Style --------
    (def style
      {:override_text_style "MyStyle"
      :text_styles
      [{:name "MyStyle"
        :size (float 46)
        :family "Monospace"}]
      :visuals
      {:override_text_color (color 250 250 250)}})

    ;;---------- main-wire ------------
    (defloop main-wire
      (Setup
      (initialize-character)
      (initialize-coin)
      (initialize-game-elements)
      (initialize-spiked-canonballs))

      (coin-gravity-logic .coiny-1 .coinx-1 .coin-velocity-1 .coin-position-1)
      (coin-gravity-logic .coiny-2 .coinx-2 .coin-velocity-2 .coin-position-2)

      (run-logic)
      (gravity-logic)
      (scoring)
      (damaging)

      (Step idle-animation)
      (Step walking-animation)

      (Step coin-animation)
      (Step random-coin-1)
      (Step random-coin-2)

      (Step spiked-canonball-animation)
      (spikeball-gravity-logic .spikeball-y-1 .spikeball-velocity-1 .spikeball-position-1 .spikeball-x-1)
      (Step  spikeball-1)

      (GFX.MainWindow
      :Title "MainWindow" :Width 1920 :Height 1080
      :Contents
      (-> (Setup
            (GFX.DrawQueue) >= .ui-draw-queue
            (GFX.UIPass .ui-draw-queue) >> .render-steps)
          .ui-draw-queue (GFX.ClearQueue)

          (UI
            .ui-draw-queue
            (->
            (UI.Area :Position .character-position
                      :Anchor Anchor.Top
                      :Contents (->
                                .character-state
                                (Match [0 (-> .character-direction
                                              (Match [0 (-> .idle-left-image-array (Take .idle-image-index) (UI.Image :Scale (float2 0.2)))
                                                      1 (-> .idle-right-image-array (Take .idle-image-index) (UI.Image :Scale (float2 0.2)))]
                                                      :Passthrough false))
                                        1 (-> .walking-left-image-array (Take .walking-image-index) (UI.Image :Scale (float2 0.2)))
                                        2 (-> .walking-right-image-array (Take .walking-image-index) (UI.Image :Scale (float2 0.2)))
                                        3 (->  .character-direction
                                                (Match [0 (-> .character-jumping-left (UI.Image :Scale (float2 0.2)))
                                                        1 (-> .character-jumping-right (UI.Image :Scale (float2 0.2)))]
                                                      :Passthrough false))]
                                        :Passthrough false)))

            (UI.Area :Position .coin-position-1
                      :Anchor Anchor.Top
                      :Contents (->
                                .coin-image-array (Take .coin-image-index) (UI.Image :Scale (float2 0.2))))

            (UI.Area :Position .coin-position-2
                      :Anchor Anchor.Top
                      :Contents (->
                                .coin-image-array (Take .coin-image-index) (UI.Image :Scale (float2 0.2))))

            (UI.Area :Position .spikeball-position-1
                      :Anchor Anchor.Top
                      :Contents (->
                                .spikeball-array (Take .spikeball-index) (UI.Image :Scale (float2 0.15))))

            (UI.Area :Position (float2 -40 20)
                      :Anchor Anchor.TopRight
                      :Contents (->
                                style (UI.Style)
                                .score (ToString) (UI.Label)))))

          (button-inputs)

          (GFX.Render :Steps .render-steps))))


    (defmesh main)
    (schedule main main-wire)
    (run main (/ 1.0 60))
    ```

Now that we have our spiked cannonball system up, we can create as many as we want. And since we already set up our shards to take in arguments, this time it would be much easier!

Create more spiked cannonballs.

All we have to do is to follow these easy steps:

1. Create the variables for our new spiked cannonball.

2. Create new `UI.Area` for each new spiked cannonball.

3. Add in `spikeball-gravity-logic` to ensure it falls.

4. Add in `randomise_spikeball_logic` to ensure that it randomizes.

5. Ensure that Glod get's damaged whenever he gets hit.

=== "Code Added"
    
    ```{.clojure .annotate linenums="1"}
    ;; ---------- spikeball-2 -------------
    1.0 >= .spikeball-velocity-2
    0.0 >= .spikeball-y-2
    0.0 >= .spikeball-x-2
    (float2 .spikeball-x-2 .spikeball-y-2) >= .spikeball-position-2

    ;; ---------- SpikeBall_3 -------------
    1.0 >= .spikeball-velocity-3
    0.0 >= .spikeball-y-3
    0.0 >= .spikeball-x-3
    (float2 .spikeball-x-3 .spikeball-y-3) >= .spikeball-position-3
    ```

    >  Step 1 ! Create the variables

    ```{.clojure .annotate linenums="1"}
    (UI.Area :Position .spikeball-position-2
                      :Anchor Anchor.Top
                      :Contents (->
                                .spikeball-array (Take .spikeball-index) (UI.Image :Scale (float2 0.15))))

    (UI.Area :Position .spikeball-position-3
                      :Anchor Anchor.Top
                      :Contents (->
                                .spikeball-array (Take .spikeball-index) (UI.Image :Scale (float2 0.15))))
    ```

    > Step 2 ! Create the UI.Area

    ```{.clojure .annotate linenums="1"}
    (spikeball-gravity-logic .spikeball-y-2 .spikeball-velocity-2 .spikeball-position-2 .spikeball-x-2)
    (spikeball-gravity-logic .spikeball-y-3 .spikeball-velocity-3 .spikeball-position-3 .spikeball-x-3)
    ```

    > Step 3 ! Add the Gravity logic

    ```{.clojure .annotate linenums="1"}
    (defloop spikeball-2
      (randomise-spikeball .spikeball-x-2 .spikeball-y-2 .spikeball-velocity-2 .spikeball-position-2 3))
    (defloop spikeball-3
      (randomise-spikeball .spikeball-x-3 .spikeball-y-3 .spikeball-velocity-3 .spikeball-position-3 4))
    ```

    ```{.clojure .annotate linenums="1"}
    (Step  spikeball-2)
    (Step  spikeball-3)
    ```

    > Step 4 ! Add the Randomise logic!

    ```{.clojure .annotate linenums="1"}
    (spikeBall-collision-logic .spikeball-x-2 .spikeball-y-2)
    (spikeBall-collision-logic .spikeball-x-3 .spikeball-y-3)
    ```
    > Step 5 ! Add the collision logic (remember this is in our damaging shard)

=== "Full Code So Far"
    
    ```{.clojure .annotate linenums="1"}
    (defshards LoadTexture [name]
      (LoadImage name)
      (GFX.Texture))

    (defshards initialize-character []
      (LoadTexture "GlodImages/Character1_Jumping_Left.png") = .character-jumping-left
      (LoadTexture "GlodImages/Character1_Jumping_Right.png") = .character-jumping-right

      0 >= .character-state
      0 >= .character-direction
      true >= .can-jump

      0.0 >= .X
      620.0 >= .Y
      (float2 .X .Y) >= .character-position
      0.0 >= .character-x-velocity
      0.0 >= .character-y-velocity
      0.0 >= .character-y-acceleration

      ;; ---------- Character Idle Array (Facing Left) ----------
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_1.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_2.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_3.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_4.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_5.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_6.png") >> .idle-left-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Left/Character1_Idle_Left_7.png") >> .idle-left-image-array

      ;; ---------- Character Idle Array (Facing Right) ----------------
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_1.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_2.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_3.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_4.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_5.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_6.png") >> .idle-right-image-array
      (LoadTexture "GlodImages/Character_Idle/Idle_Right/Character1_Idle_7.png") >> .idle-right-image-array

      0 >= .idle-image-index
      (Count .idle-left-image-array) = .idle-image-index-max
      0.08 = .idle-animation-speed

      ;; -------------- Walking Array (Facing Left) -----------------
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_1.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_2.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_3.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_4.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_5.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_6.png") >> .walking-left-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Left/Character1_Walking_Left_7.png") >> .walking-left-image-array

      ;; ----------- Walking Array (Facing Right) ---------------
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_1.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_2.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_3.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_4.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_5.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_6.png") >> .walking-right-image-array
      (LoadTexture "GlodImages/Character_Walking/Walking_Right/Character1_Walking_Right_7.png") >> .walking-right-image-array

      (Count .walking-left-image-array) = .walking-image-index-max
      0 >= .walking-image-index
      0.08 = .walking-animation-speed) ;; Reduce number to increase animation speed

    ;; --------- Idle Animation Loop ---------
    (defloop idle-animation
      .idle-image-index (Math.Add 1)
      > .idle-image-index
      (When :Predicate (IsMoreEqual .idle-image-index-max)
            :Action (-> 0 > .idle-image-index))
      (Pause .idle-animation-speed))

    ;; -------- Walking Animation Loop --------
    (defloop walking-animation
      .walking-image-index (Math.Add 1)
      > .walking-image-index
      (When :Predicate (IsMoreEqual .walking-image-index-max)
            :Action (-> 0 > .walking-image-index))
      (Pause .walking-animation-speed))

    ;; ---------- character-boundary ------------

    (defshards clamp [var min max]
      var (Max min) (Min max) > var)

    ;; ------------ Character Run Logic ----------------
    (defshards run-logic []
      .X (Math.Add .character-x-velocity)
      > .X

      (float2 .X .Y) > .character-position

      (clamp .X -600.0 600.0))

    ;; ------------ Character gravity-logic ---------------
    (defshards gravity-logic []
      .Y (Math.Add .character-y-velocity)
      > .Y

      .character-y-velocity (Math.Add .character-y-acceleration)
      > .character-y-velocity

      (float2 .X .Y) > .character-position

      (clamp .Y -620.0 620.0)
      .Y
      (When :Predicate (IsMoreEqual 620.0)
            :Action (->
                    0.0 > .character-y-velocity
                    0.0 > .character-y-acceleration
                    true > .can-jump
                    .character-state
                    (When :Predicate (Is 3)
                          :Action (->
                                    0 > .character-state)))))

    ;; ------- Button Inputs ----------
    (defshards button-inputs []
      (Inputs.KeyDown
      :Key "left"
      :Action (->
                (Msg "left")

                .character-state
                (When :Predicate (Is 0)
                      :Action (-> 1 > .character-state))

                0 > .character-direction
                -5.0 > .character-x-velocity))

      (Inputs.KeyDown
      :Key "right"
      :Action (->
                (Msg "right")

                .character-state
                (When :Predicate (Is 0)
                      :Action (-> 2 > .character-state))
                1 > .character-direction
                5.0 > .character-x-velocity))

      (Inputs.KeyDown
      :Key "up"
      :Action (->
                (Msg "up")
                3 > .character-state
                .can-jump
                (When :Predicate (Is true)
                      :Action (->
                              -20.0 > .character-y-velocity
                              1.0 >  .character-y-acceleration
                              false >= .can-jump))))

      (Inputs.KeyUp
      :Key "left"
      :Action (->
                0 > .character-state
                0.0 > .character-x-velocity))

      (Inputs.KeyUp
      :Key "right"
      :Action (->
                0 > .character-state
                0.0 > .character-x-velocity)))

    ;; -------------- Initialize Coin ----------
    (defshards initialize-coin []
      (LoadTexture "GlodImages/Coin/Coin_1.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_2.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_3.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_4.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_5.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_6.png") >> .coin-image-array
      (LoadTexture "GlodImages/Coin/Coin_7.png") >> .coin-image-array
      (Count .coin-image-array) = .coin-image-index-max
      0 >= .coin-image-index
      0.1 = .coin-animation-speed

      ;; ----- Coin 1 ------
      0.0 >= .coinx-1
      0.0 >= .coiny-1
      (float2 .coinx-1 .coiny-1) >= .coin-position-1
      0.0 >= .coin-velocity-1
      0.5 >= .coin-acceleration

      ;; ----- Coin 2 ----
      0.0 >= .coinx-2
      0.0 >= .coiny-2
      (float2 .coinx-2 .coiny-2) >= .coin-position-2
      0.0 >= .coin-velocity-2)

    ;; -------------- Coin Animation ------------------
    (defloop coin-animation
      .coin-image-index (Math.Add 1)
      > .coin-image-index
      (When :Predicate (IsMoreEqual .coin-image-index-max)
            :Action (-> 0 > .coin-image-index))

      (Pause .coin-animation-speed))

    ;; ------------- Coin Gravity ------------------
    (defshards coin-gravity-logic [coiny coinx coin-velocity coin-position]

      coiny (Math.Add coin-velocity)
      > coiny

      coin-velocity (Math.Add .coin-acceleration)
      > coin-velocity

      (float2 coinx coiny) > coin-position)

    ;; ------------- Random Coin ------------------
    (defshards random-coin [coinx coiny coin-velocity coin-position pause-length]
      coinx
      (RandomFloat :Max 1200.0)
      > coinx
      (Math.Subtract 600.0)
      > coinx

      0.0 > coiny
      0.0 > coin-velocity
      (float2 coinx coiny) > coin-position
      (Pause pause-length))

    (defloop random-coin-1
      (random-coin .coinx-1 .coiny-1 .coin-velocity-1 .coin-position-1 1.5))

    (defloop random-coin-2
      (random-coin .coinx-2 .coiny-2 .coin-velocity-2 .coin-position-2 2.5))

    ;; ------------ Initialize Spiked CanonBalls ---------------
    (defshards initialize-spiked-canonballs []
      (LoadTexture "GlodImages/SpikeBall/SpikeBall1.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall2.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall3.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall4.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall5.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall6.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall7.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall8.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall9.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall10.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall11.png") >> .spikeball-array
      (LoadTexture "GlodImages/SpikeBall/SpikeBall12.png") >> .spikeball-array

      (Count .spikeball-array) = .spikeball-array-index-max
      0 >= .spikeball-index
      0.06 = .spikeball-animation-speed

      ;; ---------- spikball-1 -------------
      1.0 >= .spikeball-velocity-1
      0.0 >= .spikeball-y-1
      0.0 >= .spikeball-x-1
      (float2 .spikeball-x-1 .spikeball-y-1) >= .spikeball-position-1

      ;; ---------- spikeball-2 -------------
      1.0 >= .spikeball-velocity-2
      0.0 >= .spikeball-y-2
      0.0 >= .spikeball-x-2
      (float2 .spikeball-x-2 .spikeball-y-2) >= .spikeball-position-2

    ;; ---------- SpikeBall_3 -------------
      1.0 >= .spikeball-velocity-3
      0.0 >= .spikeball-y-3
      0.0 >= .spikeball-x-3
      (float2 .spikeball-x-3 .spikeball-y-3) >= .spikeball-position-3

      0.5 >= .spikeball-acceleration)

    ;;------------- Spiked CanonBall Animation -------------
    (defloop spiked-canonball-animation
      .spikeball-index (Math.Add 1)
      > .spikeball-index
      (When :Predicate (IsMoreEqual .spikeball-array-index-max)
            :Action (-> 0 > .spikeball-index))

      (Pause .spikeball-animation-speed))

    ;; ------------- SpikeBall_Gravity_Logic -------------
    (defshards spikeball-gravity-logic [spikeball-y spikeball-velocity spikeball-position spikeball-x]
      spikeball-y (Math.Add spikeball-velocity)
      > spikeball-y
      spikeball-velocity (Math.Add .spikeball-acceleration)
      > spikeball-velocity
      (float2 spikeball-x spikeball-y) > spikeball-position)

    ;; ------------- Randomise Spikeball ----------------
    (defshards randomise-spikeball [spikeball-x spikeball-y spikeball-velocity spikeball-position pausefloat]
      spikeball-x
      (RandomFloat :Max 1200.0)
      > spikeball-x
      (Math.Subtract 600.0)
      > spikeball-x

      0.0 > spikeball-y
      0.0 > spikeball-velocity
      (float2 spikeball-x spikeball-y) > spikeball-position
      
      (Pause pausefloat))

    (defloop spikeball-1
      (randomise-spikeball .spikeball-x-1 .spikeball-y-1 .spikeball-velocity-1 .spikeball-position-1 2))
    (defloop spikeball-2
      (randomise-spikeball .spikeball-x-2 .spikeball-y-2 .spikeball-velocity-2 .spikeball-position-2 3))
    (defloop spikeball-3
      (randomise-spikeball .spikeball-x-3 .spikeball-y-3 .spikeball-velocity-3 .spikeball-position-3 4))

    ;; --------- Game Elements ------------
    (defshards initialize-game-elements []
      ;;------------ Scoring Limits ----------
      0 >= .score
      false >= .scored

      .X (Math.Add 50.0)
      >= .scoringUpper-x-limit
      .X (Math.Subtract 50.0)
      >= .scoringLower-x-limit

      .Y (Math.Add 10.0)
      >= .scoringUpper-y-limit
      .Y (Math.Subtract 10.0)
      >= .scoringLower-y-limit

      ;; ---------- Damage Limits ------------
      .X (Math.Add 50.0)
      >= .damageUpper-x-limit
      .X (Math.Subtract 50.0)
      >= .damageLower-x-limit

      .Y (Math.Add 5.0)
      >= .damageUpper-y-limit
      .Y (Math.Subtract 5.0)
      >= .damageLower-y-limit

      false >= .damaged)

    ;; --------- Scoring ----------
    (defshards score-collision [coinx coiny]
      coinx
      (When :Predicate (->
                        (IsLess .scoringUpper-x-limit)
                        (And)
                        coinx (IsMore .scoringLower-x-limit)
                        (And)
                        coiny (IsLess .scoringUpper-y-limit)
                        (And)
                        coiny (IsMore .scoringLower-y-limit)
                        (And)
                        .scored (Is false))
            :Action (->
                    true > .scored
                    (Log "Score: "))))

    (defshards scoring []
      .X (Math.Add 50.0)
      > .scoringUpper-x-limit
      .X (Math.Subtract 50.0)
      > .scoringLower-x-limit

      .Y (Math.Add 10.0)
      > .scoringUpper-y-limit
      .Y (Math.Subtract 10.0)
      > .scoringLower-y-limit

      (score-collision .coinx-1 .coiny-1)
      (score-collision .coinx-2 .coiny-2)

      .scored
      (When :Predicate (Is true)
            :Action (->
                    .score (Math.Add 1)
                    > .score
                    false > .scored)))

    ;; ------------- spikeBall-collision-logic --------------
    (defshards spikeBall-collision-logic [spikeBall-x spikeBall-y]

      spikeBall-x
      (If :Predicate (-> (IsLess .damageUpper-x-limit)
                        (And)
                        spikeBall-x (IsMore .damageLower-x-limit)
                        (And)
                        spikeBall-y (IsLess .damageUpper-y-limit)
                        (And)
                        spikeBall-y (IsMore .damageLower-y-limit))

          :Then (-> .damaged
                    (When :Predicate (Is false)
                          :Action (->
                                  true > .damaged
                                  (Log "damaged: "))))))

    ;; -------------- Damaging --------------
    (defshards damaging []

      .X (Math.Add 120.0)
      > .damageUpper-x-limit
      .X (Math.Subtract 120.0)
      > .damageLower-x-limit

      .Y (Math.Add 15.0)
      > .damageUpper-y-limit
      .Y (Math.Subtract 15.0)
      > .damageLower-y-limit

      (spikeBall-collision-logic .spikeball-x-1 .spikeball-y-1)
      (spikeBall-collision-logic .spikeball-x-2 .spikeball-y-2)
      (spikeBall-collision-logic .spikeball-x-3 .spikeball-y-3)

      .damaged
      (When :Predicate (Is true)
            :Action (->
                    .score (Math.Subtract 1)
                    > .score
                    false > .damaged)))



    ;; ------ UI Style --------
    (def style
      {:override_text_style "MyStyle"
      :text_styles
      [{:name "MyStyle"
        :size (float 46)
        :family "Monospace"}]
      :visuals
      {:override_text_color (color 250 250 250)}})

    ;;---------- main-wire ------------
    (defloop main-wire
      (Setup
      (initialize-character)
      (initialize-coin)
      (initialize-game-elements)
      (initialize-spiked-canonballs))

      (coin-gravity-logic .coiny-1 .coinx-1 .coin-velocity-1 .coin-position-1)
      (coin-gravity-logic .coiny-2 .coinx-2 .coin-velocity-2 .coin-position-2)

      (run-logic)
      (gravity-logic)
      (scoring)
      (damaging)

      (Step idle-animation)
      (Step walking-animation)

      (Step coin-animation)
      (Step random-coin-1)
      (Step random-coin-2)
      
      (Step spiked-canonball-animation)
      (spikeball-gravity-logic .spikeball-y-1 .spikeball-velocity-1 .spikeball-position-1 .spikeball-x-1)
      (spikeball-gravity-logic .spikeball-y-2 .spikeball-velocity-2 .spikeball-position-2 .spikeball-x-2)
      (spikeball-gravity-logic .spikeball-y-3 .spikeball-velocity-3 .spikeball-position-3 .spikeball-x-3)
      (Step  spikeball-1)
      (Step  spikeball-2)
      (Step  spikeball-3)

      (GFX.MainWindow
      :Title "MainWindow" :Width 1920 :Height 1080
      :Contents
      (-> (Setup
            (GFX.DrawQueue) >= .ui-draw-queue
            (GFX.UIPass .ui-draw-queue) >> .render-steps)
          .ui-draw-queue (GFX.ClearQueue)

          (UI
            .ui-draw-queue
            (->
            (UI.Area :Position .character-position
                      :Anchor Anchor.Top
                      :Contents (->
                                .character-state
                                (Match [0 (-> .character-direction
                                              (Match [0 (-> .idle-left-image-array (Take .idle-image-index) (UI.Image :Scale (float2 0.2)))
                                                      1 (-> .idle-right-image-array (Take .idle-image-index) (UI.Image :Scale (float2 0.2)))]
                                                      :Passthrough false))
                                        1 (-> .walking-left-image-array (Take .walking-image-index) (UI.Image :Scale (float2 0.2)))
                                        2 (-> .walking-right-image-array (Take .walking-image-index) (UI.Image :Scale (float2 0.2)))
                                        3 (->  .character-direction
                                                (Match [0 (-> .character-jumping-left (UI.Image :Scale (float2 0.2)))
                                                        1 (-> .character-jumping-right (UI.Image :Scale (float2 0.2)))]
                                                      :Passthrough false))]
                                        :Passthrough false)))

            (UI.Area :Position .coin-position-1
                      :Anchor Anchor.Top
                      :Contents (->
                                .coin-image-array (Take .coin-image-index) (UI.Image :Scale (float2 0.2))))

            (UI.Area :Position .coin-position-2
                      :Anchor Anchor.Top
                      :Contents (->
                                .coin-image-array (Take .coin-image-index) (UI.Image :Scale (float2 0.2))))

            (UI.Area :Position .spikeball-position-1
                      :Anchor Anchor.Top
                      :Contents (->
                                .spikeball-array (Take .spikeball-index) (UI.Image :Scale (float2 0.15))))

            (UI.Area :Position .spikeball-position-2
                      :Anchor Anchor.Top
                      :Contents (->
                                .spikeball-array (Take .spikeball-index) (UI.Image :Scale (float2 0.15))))

            (UI.Area :Position .spikeball-position-3
                      :Anchor Anchor.Top
                      :Contents (->
                                .spikeball-array (Take .spikeball-index) (UI.Image :Scale (float2 0.15))))

            (UI.Area :Position (float2 -40 20)
                      :Anchor Anchor.TopRight
                      :Contents (->
                                style (UI.Style)
                                .score (ToString) (UI.Label)))))

          (button-inputs)

          (GFX.Render :Steps .render-steps))))


    (defmesh main)
    (schedule main main-wire)
    (run main (/ 1.0 60))
    ```

## Recap

Good job in reaching this far!

In this chapter, we created falling spiked cannonballs using the same logic as our coins.

In the next chapter, we will round off and finish the game by adding a few more elements that will be the icing on the cake for our game.

Almost there! 😀 

See you in the next chapter.
