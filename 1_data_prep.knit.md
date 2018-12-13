---
title: "Madden Games Sales and Review Sentiments"
output: 
  md_document:
    variant: markdown_github


```r
library(stringr)
library(plyr)
```

# Clean up reviews data


```r
# metacritic
metacritic_data <- read.delim("../Data_WZ/madden_metacritic_reviews.tsv")
```

```
## Warning in scan(file = file, what = what, sep = sep, quote = quote, dec =
## dec, : EOF within quoted string
```

```r
head(metacritic_data)
```

```
##        User         Date
## 1     Steve Jun 29, 2005
## 2   JamesJ. Jan  3, 2002
## 3     Mikey Mar  3, 2002
## 4 MichaelC. Aug 23, 2001
## 5 MichaelC. Aug 23, 2001
## 6 MichaelC. Aug 23, 2001
##                                                                                                                                                                  Body
## 1                 Incredible! I bought this title when it was new, and I still play it. You can pick this game up now for a few dollars...do it, you won't regret it.
## 2                                                                                                                       It is the best sports game out on the market.
## 3  Every year Madden gets better and better. The game play is excellent, and the Franchise Mode, with the Texans is just awesome. Culpepper to Moss can't be stopped.
## 4                     After playing every football game since \\1st & 10\\ in the early 80's, Madden 2002 for PS2 is by far the best football game ever. A must-have.
## 5                     After playing every football game since \\1st & 10\\ in the early 80's, Madden 2002 for PS2 is by far the best football game ever. A must-have.
## 6                     After playing every football game since \\1st & 10\\ in the early 80's, Madden 2002 for PS2 is by far the best football game ever. A must-have.
##   Rating Console         Version
## 1     10     PS2 Madden NFL 2001
## 2     10     PS2 Madden NFL 2002
## 3     10     PS2 Madden NFL 2002
## 4     10     PS2 Madden NFL 2002
## 5     10     PS2 Madden NFL 2002
## 6     10     PS2 Madden NFL 2002
```

```r
metacritic_lines <- readLines("../Data_WZ/madden_metacritic_reviews.tsv")
new_lines <- c()
newline <- str_trim(metacritic_lines[1])
i <- 2
while(i<=length(metacritic_lines)){
    line = str_trim(metacritic_lines[i])
    if(line==""){
        newline <- paste(newline,line)
    } else {
        if(substr(line,1,1)=="\""){
            new_lines <- c(new_lines,newline)
            newline <- line
        } else {
            newline <- paste(newline,line)
        }
    }
    i <- i + 1
}
new_lines <- c(new_lines,newline)
writeLines(new_lines, con = "madden_metacritic_reviews_clean.tsv")
rm(metacritic_data)

# amazon
amazon_data <- read.delim("../Data_WZ/madden_amazon_reviews.tsv")
```

```
## Warning in scan(file = file, what = what, sep = sep, quote = quote, dec =
## dec, : EOF within quoted string
```

```r
head(amazon_data)
```

```
##                Date        Author
## 1  November 9, 2000     punkviper
## 2  January 23, 2001       Chyll57
## 3  October 31, 2000 Randal Cooper
## 4  November 2, 2000      J. Feira
## 5 November 14, 2000          Ryan
## 6  October 30, 2000         Cainz
##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       Body
## 1 At first I wasn't going to bother reviewing Madden2K1 for PS2 since everyone else has basically voiced the typical opinions.  But, as someone who remembers as far back as Madden '92, I figured I would add some insight in terms of helpful hints on gameplay.RUNNING: So unbelievably diverse.  You have to know WHEN to run, and when NOT to run.  You can't always do some cheap HB pitch to the outside, then outrun a host of chubby linebackers for a 20 yard gain.  You have to keep the defense on their toes with a good mix of running and passing plays.  Don't be afraid to run up the middle.  It works just as well as an outside run, and often better.  If you have a good fullback, don't be afraid to use him.  Deflecting tackles is just as good as outrunning a defender to get yardage.  Use the various peripheral moves like the stiff-arm and the hurdle to foil potential tacklers, because they do work.  And keep in mind that a running back that bolts into the secondary often does so uncovered, so when 3 DBs are crowding Keyshawn, don't be surprised if Warrick Dunn is left alone for some easy yards.  Use motion!  If you are running outside, don't hesitate to put the fullback in motion to that side to obtain an early block, and feel free to do the same with receivers.PASSING: Remember the success of a good short-passing game.  Not every ball has to be a 30+ yard fly route.  Especially if your QB isn't Joe Namath, you might have greater success picking apart a defense with comeback routes and sideline passes.  The opposing pass defense is NOT infallible.  Watch their eyes light up in horror as on 3rd and 1 you audible into a shotgun streak play and your 4 fastest receivers go sprinting into a depleted secondary.  Remember the velocity of the pass.  There is a time to lob and there is a time to bullet.  If you see coverage on a short pass, don't be afraid to put some oomph on the pass to fit it in there.  If you are facing some rather large defensive linemen bearing down on your QB, but your receiver hasn't \\gotten there\\ yet, you will need to lob.  Don't be afraid to go deep with a good receiver.  It might not always work, but be sure to pick the best receiver available when you do.  The ability to cancel a pass whilst scrambling is very useful.  If you drop back to pass and get forced out of the pocket, hit the triangle button to stowe the passing icons and hustle it upfield.  You can always toggle them back on if you find a receiver before you cross the line of scrimmage.DEFENSE: When in the secondary, be extra careful if you are taking control of a defensive back.  It is one of the easiest ways to kill yourself on defense if you quickly switch control to the covering defender, only to screw up and have the receiver outsprint you for a huge gain.  I have given up many a long pass due to incompetance that could have been prevented if I had just let the computer handle the coverage for me.  Don't be a hero!  When on the defensive line, don't forget to utilize the shoulder buttons to get past the opposing line.  It's more effective than just pushing down and hoping the other guy commits an error.  Linebacker is a fun choice to control on defense, but just be wary of that player's coverage responsibilities.  You might find rushing the QB to be a good way to hurry the pass, but when the QB finds an open receiver courtesy of the coverage you failed to put on him, it burns.  As in most football games, concentrate on stopping the pass, because stuffing the run is not that difficult, even in pass coverage formations.KICKING: Don't take field goals for granted!  The field goal kicking in this game can be merciless.  If it's over 40 yards, you really need to make sure you can control your kicker's momentum gauge, or else you will be looking mighty pathetic.  Point-afters are practically automatic, as they should be, and punting is generally not as nerve-wracking as field goal kicking.  Remember to use the field when punting, don't hesitate to aim for the sides of the field.I owned NFL2K1 for DC, and I own Madden2K1 on PS2.  Both are quality games, and you really can't go wrong with either, but I find Madden to be more fulfilling.  The atmosphere is nicer, the rolling stats of the play-calling page along with the post-play TV style presentation are great ways to add to the substance of the game.  The players models look more correct, and less like giant zombies, though faces occasionally look like circus kewpie dolls.  Madden comes off as a deeper game with more substance (let's not forget the Madden Cards.)
## 2                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          After reading every review posted in the hopes of getting as much information as possible (not to mention trying to get some tips), I think I am ready to weigh in on the great Madden 2001 vs. NFL2K1 debate. First of all, as an owner of both games, systems and having played football myself, I am constantly struggling to strike the balance between superior graphices and realistic gameplay.  Both of these titles have some of the critical elements and yet both are flawed as well.  Let's go to the head-to-head matchups.Graphics: Both titles offer some of the best in the genre, but I have some problems with Madden.  The replay graphics are marvelous, but the actual graphics during gameplay are not superior to 2K1.  2K1's figures are larger and well defined, and Madden's figures from a distance look too short and stubby for certain positions (i.e. defensive back).  Edge: even (Madden's up close shots make up for any other flaws).Gameplay: 2K1 did well to improve on the ability to run the ball and jumpstart offensive play, but the fact that a scrambling QB can drop back 40 yards and still chuck a seventy-yarder with a man all over him takes away from the realism. With Madden you have to step up in the pocket, set your feet, and then decide whether to lob the ball or unleash a rocket. You can run the ball but always be prepared to bounce to where the hole opens up and to hop over outstretched limbs and body parts.  Defensively, 2K1 plays better pass defense with the buttons, but the better defensive linemen eat up QBs at a ridiculous rate.  Madden's sack totals are more realistic and pressure on the QB can cause strange and realistic things to happen.  Your best bet is to let the computer help your DBs and scheme to shut down the passing game.  Edge: Madden (the more you play, the more you figure out).Extras:  Here is where Madden blows away 2K1.  Sega has the early lead in online play, but you know it will be here soon for PS2. Other than that and an edge in the quality of the  commentary (Madden & Summerall are disappointing, Visser is terrible!), it is all Madden.  Interactive sidelines, receivers trying stay inbounds, off-balance throws and spin moves, coaches challenges, haggling with agents during franchise play, the correct dynamics of physics and much more blow away all the competition.  Huge edge here for Madden.Overall: Like any new system rushed to beat a timeline, Madden 2001 has its flaws, but you have to believe it will only get better.  Even with the glitches it is an awesome debut for a great title.  You won't regret the buy.  Sorry this was so long but I hope it helps.
## 3                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           I have never seen a game attract the attention of forty-year-old men the way a demo of Madden 2001 on PS2 did at Best Buy the other day. Then again, that may be the only place they could see it in action, given the hardware shortage.Graphically, the game is fantastic--though not perfect. Tackles, replays, cut scenes, and player animations all surpass what has come before (By light years on PS1, and by a significant margin on Dreamcast's NFL2K1, as well). The player detail is unmatched. On the other hand, huddle and sideline shots become repetitive quickly, and players can pass through one another during after-play cut scenes. Neither affects gameplay, however.The gameplay is there in spades. The computer AI is strong enough to prevent the game from becoming too easy, but not so tenacious as to become discouraging for a novice (which, admittedly, I am). There are some minor control issues, where you don't feel you have one hundred percent control of your quarterback one hundred percent of the time, and occasionally you wonder if your offensive line was aware of the play that you called, but these epsiodes are rare.I have mixed emotions about the sound. While the play-by-play is adequate, Madden's color commentary is repetitive to the point of being irksome. It is nice, however, to hear your name being called after setting up your virtural doppelganger in the create-a-player mode (I guess it helps to have a common last name).The good news is that Madden 2001 is a showcase title for the PS2, in spite of its very minor shortcomings. The better news is that Madden 2002 could conceivably improve on this game in every aspect, adding online play, improved commentary, and fixing the rare graphical glitch. In the meatime, I'm keeping an eye out for Dennis Miller Football--(Insert Vague Metaphor Here).
## 4                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            Best football game I've ever played.GRAPHICS - almost perfect. Collision detection is a little off sometimes (player falls before graphics show the collision), and occasional glitches (player's helmet goes through another player) but overall amazingSOUND - very bad! John Madden and Pat Summerall are terrible announcers - no emotion in Pat's voice and Madden's phrases are repeated far too often. One time Madden repeated the same phrase on 3 straight playsGAMEPLAY - perfect. The game is a little slower than the PS1 version, which actually makes it better. More time to read blocks and really appreciate the graphicsOVERALL - great game. Great replayability with Franchise Mode and Madden cards (earn them by accomplishing various challenges, then trade with friends). Just fix the commentary and graphics glitches and this is a perfect game.
## 5                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      MADDEN 2001 1 or 2 playersThis is the best football game I have ever seen. If you thought NFL 2K was good for the Sega Dreamcast, wait until you play this game; it will blow you away.  As you play the game you notice that unlike most other football games, the players get grass stains on their uniforms, their eyes blink, they talk with one another, and sometimes they even get grass stuck in the sides of their helmets! It is very realistic.  You can pick from any of the current NFL teams their are today or you can choose great historical teams from the past. There are many types of gameplay within the game also such as an exibition game, a season, or just have a team practice. If you choose a season you can pick your favorite team and work your way to the super bowl. Along the way you will break records and earn madden challenge points. With these points you can unlock secret stadiums, cheats, and even players of the past such as Walter Payton.  If you are thinking about a game to buy for your Sony Playstaion 2 I would highly recomend it. But if you do choose this game make sure that the primary player is at an age that they can understand the game of football. Younger children (maybe under 8 years old) may find this game hard to play. I am 15 years old and I think that this game would be great for someone around my age or older. If you choose Madden 2001 you will be happy with it, it will provide many, many hours of fun.
## 6                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            Madden 2001 is one of two Playstation 2 games that you MUST have (the other being SSX).  The only excuse there is not to have this game is if you absolutely hate football or don't understand a thing about it.  The graphics are the best out there of any football game on ANY console.  Ever.  The gameplay is simply classic Madden style of play, and the attention to detail is simply amazing, from the detailed way that players' helmets reflect images and light, to the realistic way the players run, move, tackle, juke, and catch, to the way the players uniforms get dirtier or muddier with each play.  This is game is ultra-realistic when it comes to interpreting football into a console game.  The strategy involved in playing the game, the realistic way the bone-jarring hits take place- everything about this game is simply amazing to look at.  There are many different modes and options to play in as well, which adds great replay value.  This game really displays flashes of what the Playstation 2 is capable of doing.  GET THIS GAME.  IT KICKIMUS MAXIMUS BUTIMUS.
##   Rating Console         Version
## 1      5     PS2 Madden NFL 2001
## 2      4     PS2 Madden NFL 2001
## 3      4     PS2 Madden NFL 2001
## 4      4     PS2 Madden NFL 2001
## 5      5     PS2 Madden NFL 2001
## 6      5     PS2 Madden NFL 2001
```

```r
amazon_lines <- readLines("../Data_WZ/madden_amazon_reviews.tsv")
new_lines <- c()
newline <- str_trim(amazon_lines[1])
i <- 2
while(i<=length(amazon_lines)){
    line = str_trim(amazon_lines[i])
    if(line==""){ # loop until the next line starting with a quote
        i <- i + 1
        while(i<=length(amazon_lines) & substr(line,1,1)!="\""){
            line = amazon_lines[i]
            i <- i + 1
            cat(paste(i,line,"\n"))
        }
        line = str_trim(amazon_lines[i])
        newline <- paste(newline,line)
    } else {
        if(substr(line,1,1)=="\""){ # a good new line
            new_lines <- c(new_lines,newline)
            newline <- line
        } else { # append to previous
            newline <- paste(newline,line)
        }
    }
    i <- i + 1
}
```

```
## 2651          
## 2652      
## 2653      
## 2654          
## 2655  
## 2656       P.when('review-image-binder', 'reviewsLightbox-js').execute(function(reviewImageBinder) { 
## 2657           reviewImageBinder.bindReview(\"R3BZMGAZGV90Z8\",  
## 2658               \"R3BZMGAZGV90Z8_imageSection_main\",  
## 2659               \"R3BZMGAZGV90Z8_gallerySection_main\"); 
## 2660       }); 
## 2661      
## 2662 "	1	"Xbox 360"	"Madden NFL 2009" 
## 4096          
## 4097          
## 4098          
## 4099          
## 4100          
## 4101          
## 4102          
## 4103          
## 4104      
## 4105      
## 4106          
## 4107  
## 4108       P.when('review-image-binder', 'reviewsLightbox-js').execute(function(reviewImageBinder) { 
## 4109           reviewImageBinder.bindReview(\"RKV7TINVNB69L\",  
## 4110               \"RKV7TINVNB69L_imageSection_main\",  
## 4111               \"RKV7TINVNB69L_gallerySection_main\"); 
## 4112       }); 
## 4113      
## 4114 "	5	"Xbox 360"	"Madden NFL 2013" 
## 5292          
## 5293      
## 5294      
## 5295          
## 5296  
## 5297       P.when('review-image-binder', 'reviewsLightbox-js').execute(function(reviewImageBinder) { 
## 5298           reviewImageBinder.bindReview(\"R17D6M8Y68L7MK\",  
## 5299               \"R17D6M8Y68L7MK_imageSection_main\",  
## 5300               \"R17D6M8Y68L7MK_gallerySection_main\"); 
## 5301       }); 
## 5302      
## 5303 "	5	"Xbox 360"	"Madden NFL 2015" 
## 6051          
## 6052      
## 6053      
## 6054          
## 6055  
## 6056       P.when('review-image-binder', 'reviewsLightbox-js').execute(function(reviewImageBinder) { 
## 6057           reviewImageBinder.bindReview(\"R1IST1320ZWGYL\",  
## 6058               \"R1IST1320ZWGYL_imageSection_main\",  
## 6059               \"R1IST1320ZWGYL_gallerySection_main\"); 
## 6060       }); 
## 6061      
## 6062 11 comment|8 people found this helpful. Was this review helpful to you?YesNoReport abusePlease write at least one wordYou must purchase at least one item from Amazon to post a commentA problem occurred while submitting your comment. Please try again later.Sign in and comment 
## 6063     Showing 0 of"	1	"Xbox 360"	"Madden NFL 2016" 
## 6064 "November 9, 2015"	"John Torrence"	"No real upgrades to speak of from NFL 15. Just wanted current roster players etc. If you want most accurate roster they have you with or without significant upgrades. Did get it on goldbox deal though so it was a good price, wouldn't pay retail if it can be avoided. Game itself is great!"	4	"Xbox 360"	"Madden NFL 2016" 
## 6239          
## 6240      
## 6241      
## 6242          
## 6243  
## 6244       P.when('review-image-binder', 'reviewsLightbox-js').execute(function(reviewImageBinder) { 
## 6245           reviewImageBinder.bindReview(\"R22HHXVK2AWLFK\",  
## 6246               \"R22HHXVK2AWLFK_imageSection_main\",  
## 6247               \"R22HHXVK2AWLFK_gallerySection_main\"); 
## 6248       }); 
## 6249      
## 6250 "	1	"Xbox 360"	"Madden NFL 2016" 
## 8992          
## 8993      
## 8994      
## 8995          
## 8996  
## 8997       P.when('review-image-binder', 'reviewsLightbox-js').execute(function(reviewImageBinder) { 
## 8998           reviewImageBinder.bindReview(\"RTGQ77MJQV57T\",  
## 8999               \"RTGQ77MJQV57T_imageSection_main\",  
## 9000               \"RTGQ77MJQV57T_gallerySection_main\"); 
## 9001       }); 
## 9002      
## 9003 "	5	"PS3"	"Madden NFL 25" 
## 9151          
## 9152      
## 9153      
## 9154          
## 9155  
## 9156       P.when('review-image-binder', 'reviewsLightbox-js').execute(function(reviewImageBinder) { 
## 9157           reviewImageBinder.bindReview(\"ROR4RC2FLBF7T\",  
## 9158               \"ROR4RC2FLBF7T_imageSection_main\",  
## 9159               \"ROR4RC2FLBF7T_gallerySection_main\"); 
## 9160       }); 
## 9161      
## 9162 "	5	"PS3"	"Madden NFL 2015" 
## 10448          
## 10449      
## 10450      
## 10451          
## 10452  
## 10453       P.when('review-image-binder', 'reviewsLightbox-js').execute(function(reviewImageBinder) { 
## 10454           reviewImageBinder.bindReview(\"R1U3AR67RE273L\",  
## 10455               \"R1U3AR67RE273L_imageSection_main\",  
## 10456               \"R1U3AR67RE273L_gallerySection_main\"); 
## 10457       }); 
## 10458      
## 10459 "	1	"Xbox One"	"Madden NFL 2016" 
## 10502          
## 10503          
## 10504          
## 10505      
## 10506      
## 10507          
## 10508  
## 10509       P.when('review-image-binder', 'reviewsLightbox-js').execute(function(reviewImageBinder) { 
## 10510           reviewImageBinder.bindReview(\"RE2XZ88UZBIKK\",  
## 10511               \"RE2XZ88UZBIKK_imageSection_main\",  
## 10512               \"RE2XZ88UZBIKK_gallerySection_main\"); 
## 10513       }); 
## 10514      
## 10515 "	4	"Xbox One"	"Madden NFL 2016" 
## 10535          
## 10536      
## 10537      
## 10538          
## 10539  
## 10540       P.when('review-image-binder', 'reviewsLightbox-js').execute(function(reviewImageBinder) { 
## 10541           reviewImageBinder.bindReview(\"R344QQSP01RJY\",  
## 10542               \"R344QQSP01RJY_imageSection_main\",  
## 10543               \"R344QQSP01RJY_gallerySection_main\"); 
## 10544       }); 
## 10545      
## 10546 "	1	"Xbox One"	"Madden NFL 2016" 
## 10641          
## 10642      
## 10643      
## 10644          
## 10645  
## 10646       P.when('review-image-binder', 'reviewsLightbox-js').execute(function(reviewImageBinder) { 
## 10647           reviewImageBinder.bindReview(\"RJJQH93WQMFX5\",  
## 10648               \"RJJQH93WQMFX5_imageSection_main\",  
## 10649               \"RJJQH93WQMFX5_gallerySection_main\"); 
## 10650       }); 
## 10651      
## 10652 "	5	"Xbox One"	"Madden NFL 2016" 
## 10866          
## 10867          
## 10868      
## 10869      
## 10870          
## 10871  
## 10872       P.when('review-image-binder', 'reviewsLightbox-js').execute(function(reviewImageBinder) { 
## 10873           reviewImageBinder.bindReview(\"R2TU2SQ8904779\",  
## 10874               \"R2TU2SQ8904779_imageSection_main\",  
## 10875               \"R2TU2SQ8904779_gallerySection_main\"); 
## 10876       }); 
## 10877      
## 10878 "	5	"Xbox One"	"Madden NFL 2016" 
## 10883          
## 10884      
## 10885      
## 10886          
## 10887  
## 10888       P.when('review-image-binder', 'reviewsLightbox-js').execute(function(reviewImageBinder) { 
## 10889           reviewImageBinder.bindReview(\"R2VT7298530LEP\",  
## 10890               \"R2VT7298530LEP_imageSection_main\",  
## 10891               \"R2VT7298530LEP_gallerySection_main\"); 
## 10892       }); 
## 10893      
## 10894 "	4	"Xbox One"	"Madden NFL 2016" 
## 12055          
## 12056      
## 12057      
## 12058          
## 12059  
## 12060       P.when('review-image-binder', 'reviewsLightbox-js').execute(function(reviewImageBinder) { 
## 12061           reviewImageBinder.bindReview(\"R3K5MMXNZ8KWML\",  
## 12062               \"R3K5MMXNZ8KWML_imageSection_main\",  
## 12063               \"R3K5MMXNZ8KWML_gallerySection_main\"); 
## 12064       }); 
## 12065      
## 12066 "	4	"PS4"	"Madden NFL 2016" 
## 12309          
## 12310      
## 12311      
## 12312          
## 12313  
## 12314       P.when('review-image-binder', 'reviewsLightbox-js').execute(function(reviewImageBinder) { 
## 12315           reviewImageBinder.bindReview(\"R2BS8IBQ8DZX0P\",  
## 12316               \"R2BS8IBQ8DZX0P_imageSection_main\",  
## 12317               \"R2BS8IBQ8DZX0P_gallerySection_main\"); 
## 12318       }); 
## 12319      
## 12320 "	5	"PS4"	"Madden NFL 2016" 
## 12422          
## 12423          
## 12424          
## 12425      
## 12426      
## 12427          
## 12428  
## 12429       P.when('review-image-binder', 'reviewsLightbox-js').execute(function(reviewImageBinder) { 
## 12430           reviewImageBinder.bindReview(\"R3DUDRI08ORIA3\",  
## 12431               \"R3DUDRI08ORIA3_imageSection_main\",  
## 12432               \"R3DUDRI08ORIA3_gallerySection_main\"); 
## 12433       }); 
## 12434      
## 12435 "	2	"PS4"	"Madden NFL 2016" 
## 12441          
## 12442      
## 12443      
## 12444          
## 12445  
## 12446       P.when('review-image-binder', 'reviewsLightbox-js').execute(function(reviewImageBinder) { 
## 12447           reviewImageBinder.bindReview(\"R21CLC496I4KXR\",  
## 12448               \"R21CLC496I4KXR_imageSection_main\",  
## 12449               \"R21CLC496I4KXR_gallerySection_main\"); 
## 12450       }); 
## 12451      
## 12452 "	5	"PS4"	"Madden NFL 2016"
```

```r
new_lines <- c(new_lines,newline)
writeLines(new_lines, con = "madden_amazon_reviews_clean.tsv")
rm(amazon_data)
```

# Read clean review data


```r
metacritic_data <- read.delim("madden_metacritic_reviews_clean.tsv", quote="", stringsAsFactors = FALSE)
names(metacritic_data)
```

```
## [1] "X.User."    "X.Date."    "X.Body."    "X.Rating."  "X.Console."
## [6] "X.Version."
```

```r
metacritic_data$Date <- format(as.Date(substr(metacritic_data$X.Date.,2,nchar(metacritic_data$X.Date.)-1),format="%B %d, %Y"),"%Y-%m-%d")
metacritic_data$Author <- substr(metacritic_data$X.User.,2,nchar(metacritic_data$X.User.)-1)
metacritic_data$Body <- gsub("[^[:print:]]","",metacritic_data$X.Body.)
metacritic_data$Body <- gsub("\"","",metacritic_data$Body)
metacritic_data$Body <- gsub("\t"," ",metacritic_data$Body)
metacritic_data$Rating <- as.numeric(metacritic_data$X.Rating/2)

#metacritic_data$Rating <- as.numeric(substr(metacritic_data$X.Rating.,2,nchar(metacritic_data$X.Rating.)-1))
metacritic_data$Console <- substr(metacritic_data$X.Console.,2,nchar(metacritic_data$X.Console.)-1)
metacritic_data$Version <- substr(metacritic_data$X.Version.,2,nchar(metacritic_data$X.Version.)-1)
write.table(metacritic_data[,7:12],file="metacritic_reviews.tsv", sep="\t", quote=FALSE, na="", row.names = FALSE)

amazon_data <- read.delim("madden_amazon_reviews_clean.tsv", quote="", stringsAsFactors = FALSE)
names(amazon_data)
```

```
## [1] "X.Date."    "X.Author."  "X.Body."    "X.Rating."  "X.Console."
## [6] "X.Version."
```

```r
amazon_data$Date <- format(as.Date(substr(amazon_data$X.Date.,2,nchar(amazon_data$X.Date.)-1),format="%B %d, %Y"),"%Y-%m-%d")
amazon_data$Author <- substr(amazon_data$X.Author.,2,nchar(amazon_data$X.Author.)-1)
amazon_data$Body <- gsub("[^[:print:]]","",amazon_data$X.Body.)
amazon_data$Body <- gsub("\"","",amazon_data$Body)
amazon_data$Body <- gsub("\t"," ",amazon_data$Body)
amazon_data$Body <- as.character(amazon_data$Body)
dim(amazon_data$Body)
```

```
## NULL
```

```r
amazon_data$Rating <-  as.numeric(amazon_data$X.Rating)
```

```
## Warning: NAs introduced by coercion
```

```r
amazon_data$Console <- substr(amazon_data$X.Console.,2,nchar(amazon_data$X.Console.)-1)
amazon_data$Version <- substr(amazon_data$X.Version.,2,nchar(amazon_data$X.Version.)-1)
write.table(amazon_data[,7:12],file="amazon_reviews.tsv", sep="\t", quote=FALSE, na="", row.names = FALSE)

metacritic_data <- metacritic_data[,7:12]
amazon_data <- amazon_data[,7:12]
source <- c(rep("metacritic",nrow(metacritic_data)),rep("amazon",nrow(amazon_data)))
all_reviews <- rbind(metacritic_data,amazon_data)
all_reviews <- cbind(source,all_reviews)
head(all_reviews)
```

```
##       source       Date    Author
## 1 metacritic 2005-06-29     Steve
## 2 metacritic 2002-01-03   JamesJ.
## 3 metacritic 2002-03-03     Mikey
## 4 metacritic 2001-08-23 MichaelC.
## 5 metacritic 2001-08-23 MichaelC.
## 6 metacritic 2001-08-23 MichaelC.
##                                                                                                                                                                  Body
## 1                 Incredible! I bought this title when it was new, and I still play it. You can pick this game up now for a few dollars...do it, you won't regret it.
## 2                                                                                                                       It is the best sports game out on the market.
## 3  Every year Madden gets better and better. The game play is excellent, and the Franchise Mode, with the Texans is just awesome. Culpepper to Moss can't be stopped.
## 4                     After playing every football game since \\1st & 10\\ in the early 80's, Madden 2002 for PS2 is by far the best football game ever. A must-have.
## 5                     After playing every football game since \\1st & 10\\ in the early 80's, Madden 2002 for PS2 is by far the best football game ever. A must-have.
## 6                     After playing every football game since \\1st & 10\\ in the early 80's, Madden 2002 for PS2 is by far the best football game ever. A must-have.
##   Rating Console         Version
## 1      5     PS2 Madden NFL 2001
## 2      5     PS2 Madden NFL 2002
## 3      5     PS2 Madden NFL 2002
## 4      5     PS2 Madden NFL 2002
## 5      5     PS2 Madden NFL 2002
## 6      5     PS2 Madden NFL 2002
```

```r
rm(metacritic_data)
rm(amazon_data)
```

# Score review sentiments


```r
dictDir <- "Dictionaries/"
hu.liu.pos <- scan(file.path(dictDir, 'positive-words.txt'), what='character', comment.char=';')
hu.liu.neg <- scan(file.path(dictDir, 'negative-words.txt'), what='character', comment.char=';')
score.sentiment = function(sentences, pos.words, neg.words, .progress='none'){
    scores = laply(sentences, function(sentence, pos.words, neg.words) {
        sentence = gsub('[[:punct:]]', '', sentence)
        sentence = gsub('[[:cntrl:]]', '', sentence)
        sentence = gsub('\\d+', '', sentence)
        sentence = tolower(sentence)
        word.list = str_split(sentence, '\\s+')
        words = unlist(word.list)
        pos.matches = match(words, pos.words)
        neg.matches = match(words, neg.words)
        pos.matches = !is.na(pos.matches)
        neg.matches = !is.na(neg.matches)
        score = sum(pos.matches) - sum(neg.matches)
        return(score)
    }, pos.words, neg.words, .progress=.progress )
    scores.df = data.frame(score=scores, text=sentences)
    return(scores.df)
}

# Score the sentiments in reviews
result <- score.sentiment(all_reviews$Body,  hu.liu.pos, hu.liu.neg, .progress='text')
```

```
##   |                                                                         |                                                                 |   0%  |                                                                         |                                                                 |   1%  |                                                                         |=                                                                |   1%  |                                                                         |=                                                                |   2%  |                                                                         |==                                                               |   2%  |                                                                         |==                                                               |   3%  |                                                                         |==                                                               |   4%  |                                                                         |===                                                              |   4%  |                                                                         |===                                                              |   5%  |                                                                         |====                                                             |   5%  |                                                                         |====                                                             |   6%  |                                                                         |====                                                             |   7%  |                                                                         |=====                                                            |   7%  |                                                                         |=====                                                            |   8%  |                                                                         |======                                                           |   8%  |                                                                         |======                                                           |   9%  |                                                                         |======                                                           |  10%  |                                                                         |=======                                                          |  10%  |                                                                         |=======                                                          |  11%  |                                                                         |=======                                                          |  12%  |                                                                         |========                                                         |  12%  |                                                                         |========                                                         |  13%  |                                                                         |=========                                                        |  13%  |                                                                         |=========                                                        |  14%  |                                                                         |=========                                                        |  15%  |                                                                         |==========                                                       |  15%  |                                                                         |==========                                                       |  16%  |                                                                         |===========                                                      |  16%  |                                                                         |===========                                                      |  17%  |                                                                         |===========                                                      |  18%  |                                                                         |============                                                     |  18%  |                                                                         |============                                                     |  19%  |                                                                         |=============                                                    |  19%  |                                                                         |=============                                                    |  20%  |                                                                         |=============                                                    |  21%  |                                                                         |==============                                                   |  21%  |                                                                         |==============                                                   |  22%  |                                                                         |===============                                                  |  22%  |                                                                         |===============                                                  |  23%  |                                                                         |===============                                                  |  24%  |                                                                         |================                                                 |  24%  |                                                                         |================                                                 |  25%  |                                                                         |=================                                                |  25%  |                                                                         |=================                                                |  26%  |                                                                         |=================                                                |  27%  |                                                                         |==================                                               |  27%  |                                                                         |==================                                               |  28%  |                                                                         |===================                                              |  28%  |                                                                         |===================                                              |  29%  |                                                                         |===================                                              |  30%  |                                                                         |====================                                             |  30%  |                                                                         |====================                                             |  31%  |                                                                         |====================                                             |  32%  |                                                                         |=====================                                            |  32%  |                                                                         |=====================                                            |  33%  |                                                                         |======================                                           |  33%  |                                                                         |======================                                           |  34%  |                                                                         |======================                                           |  35%  |                                                                         |=======================                                          |  35%  |                                                                         |=======================                                          |  36%  |                                                                         |========================                                         |  36%  |                                                                         |========================                                         |  37%  |                                                                         |========================                                         |  38%  |                                                                         |=========================                                        |  38%  |                                                                         |=========================                                        |  39%  |                                                                         |==========================                                       |  39%  |                                                                         |==========================                                       |  40%  |                                                                         |==========================                                       |  41%  |                                                                         |===========================                                      |  41%  |                                                                         |===========================                                      |  42%  |                                                                         |============================                                     |  42%  |                                                                         |============================                                     |  43%  |                                                                         |============================                                     |  44%  |                                                                         |=============================                                    |  44%  |                                                                         |=============================                                    |  45%  |                                                                         |==============================                                   |  45%  |                                                                         |==============================                                   |  46%  |                                                                         |==============================                                   |  47%  |                                                                         |===============================                                  |  47%  |                                                                         |===============================                                  |  48%  |                                                                         |================================                                 |  48%  |                                                                         |================================                                 |  49%  |                                                                         |================================                                 |  50%  |                                                                         |=================================                                |  50%  |                                                                         |=================================                                |  51%  |                                                                         |=================================                                |  52%  |                                                                         |==================================                               |  52%  |                                                                         |==================================                               |  53%  |                                                                         |===================================                              |  53%  |                                                                         |===================================                              |  54%  |                                                                         |===================================                              |  55%  |                                                                         |====================================                             |  55%  |                                                                         |====================================                             |  56%  |                                                                         |=====================================                            |  56%  |                                                                         |=====================================                            |  57%  |                                                                         |=====================================                            |  58%  |                                                                         |======================================                           |  58%  |                                                                         |======================================                           |  59%  |                                                                         |=======================================                          |  59%  |                                                                         |=======================================                          |  60%  |                                                                         |=======================================                          |  61%  |                                                                         |========================================                         |  61%  |                                                                         |========================================                         |  62%  |                                                                         |=========================================                        |  62%  |                                                                         |=========================================                        |  63%  |                                                                         |=========================================                        |  64%  |                                                                         |==========================================                       |  64%  |                                                                         |==========================================                       |  65%  |                                                                         |===========================================                      |  65%  |                                                                         |===========================================                      |  66%  |                                                                         |===========================================                      |  67%  |                                                                         |============================================                     |  67%  |                                                                         |============================================                     |  68%  |                                                                         |=============================================                    |  68%  |                                                                         |=============================================                    |  69%  |                                                                         |=============================================                    |  70%  |                                                                         |==============================================                   |  70%  |                                                                         |==============================================                   |  71%  |                                                                         |==============================================                   |  72%  |                                                                         |===============================================                  |  72%  |                                                                         |===============================================                  |  73%  |                                                                         |================================================                 |  73%  |                                                                         |================================================                 |  74%  |                                                                         |================================================                 |  75%  |                                                                         |=================================================                |  75%  |                                                                         |=================================================                |  76%  |                                                                         |==================================================               |  76%  |                                                                         |==================================================               |  77%  |                                                                         |==================================================               |  78%  |                                                                         |===================================================              |  78%  |                                                                         |===================================================              |  79%  |                                                                         |====================================================             |  79%  |                                                                         |====================================================             |  80%  |                                                                         |====================================================             |  81%  |                                                                         |=====================================================            |  81%  |                                                                         |=====================================================            |  82%  |                                                                         |======================================================           |  82%  |                                                                         |======================================================           |  83%  |                                                                         |======================================================           |  84%  |                                                                         |=======================================================          |  84%  |                                                                         |=======================================================          |  85%  |                                                                         |========================================================         |  85%  |                                                                         |========================================================         |  86%  |                                                                         |========================================================         |  87%  |                                                                         |=========================================================        |  87%  |                                                                         |=========================================================        |  88%  |                                                                         |==========================================================       |  88%  |                                                                         |==========================================================       |  89%  |                                                                         |==========================================================       |  90%  |                                                                         |===========================================================      |  90%  |                                                                         |===========================================================      |  91%  |                                                                         |===========================================================      |  92%  |                                                                         |============================================================     |  92%  |                                                                         |============================================================     |  93%  |                                                                         |=============================================================    |  93%  |                                                                         |=============================================================    |  94%  |                                                                         |=============================================================    |  95%  |                                                                         |==============================================================   |  95%  |                                                                         |==============================================================   |  96%  |                                                                         |===============================================================  |  96%  |                                                                         |===============================================================  |  97%  |                                                                         |===============================================================  |  98%  |                                                                         |================================================================ |  98%  |                                                                         |================================================================ |  99%  |                                                                         |=================================================================|  99%  |                                                                         |=================================================================| 100%
```

```r
all_reviews$scores <- result$score

# length of reviews
library(stringr) 
all_reviews$length <- str_length(all_reviews$Body)

#write.table(all_reviews, file="all_reviews2.tsv", sep="\t", quote=FALSE, na="", row.names = FALSE)
```
