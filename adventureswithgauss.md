# Adventures with Gauss and a partial pivot

_Written while I was a Python and Algebra tutor in 2017_

Recently a student asked if I could help her understand, step by step, the Gaussian Elimination with Partial Pivoting algorithm. 

#### a partial pivot
Personally, this experience taught me that matrix operations themselves are not too bad on pen and paper. However, the matrix operations in Python are an indexing nightmare. 

#### adventures with Gauss
The point of this blog article is to instill advice regarding matrices in Python to reduce stress, dyslexia and ultimately understand this algorithm. Here are the take home points we discussed: 

1. Ignore the code and solve the matrix using Gaussian Elimination with Partial Pivoting by hand first. This helps us visualize how the algorithm will operate. I like the videos from numericalmethodsguy the best. See, [Gaussian Elimination with Partial Pivoting](https://www.youtube.com/watch?v=euIXYdyjlqo).

1. Write down what i, j, and k equal for a few of the iterations and circle the affected items of the matrix on pen and paper. The key is to cozy up to the algorithm, really get to know it, and to not expect the matrix operations to jump out from the algorithm without a bit of effort on our end. This step reduces index induced dyslexia.

1. After we get to know the algorithm for a few iterations, forget about the indexes. Print out the matrix after every operation to see the algorithm in action and to troubleshoot the algorithm. Watching the matrix change via the printed output is easier than attempting to mentally keep track of the indexes. This step keeps us sane.

Here is a refactored and print() heavy sample we worked with:

```python
print(""Guassian Elimination with Partial Pivot
"")
#Tested Python 3

import numpy as np

#Start of parameters to change.
#Change this to any n x n matrix.
A = [[ 1,  2,  0],
     [ 0,  4, -1],
     [ 2,  2,  0]]
#End of parameters to change.

def print_matrix(A):
    return '
'.join([''.join(['{:8}'.format(i) for i in r]) for r in A])

n = len(A)
npivot = 0

print(""Original matrix:
{0}
"".format(print_matrix(A)))

#Numpy sanity check
detA = np.linalg.det(A)

for k in range(0, n):
    mx = abs(A[k][k])
    rowmax = k
    for i in range(k+1, n): 
        if abs(A[i][k]) > mx:
            mx = abs(A[i][k])
            rowmax = i
    if rowmax != k: 
        npivot += 1
        store = A[k]
        A[k]=A[rowmax]
        A[rowmax]=store

    print(""after row switch:
{0}"".format(print_matrix(A)))
    
    for i in range(k+1, n): 
        factor = A[i][k]/A[k][k] 
        for j in range(k, n): 
            A[i][j] = A[i][j] - factor*A[k][j]

    print(""after eliminating:
{0}"".format(print_matrix(A)), end=""

"")
    
print(""upper triangluar matrix:
{0}"".format(print_matrix(A)))

deter = 1
for i in range(0, n): 
    deter = deter * A[i][i]
deter = deter * (-1)**npivot

print(""
our determinant: "", deter)
print(""numpy's determinant: "", detA)
``` |null|<p>Recently a student asked if I could help her understand, step by step, the Gaussian Elimination with Partial Pivoting algorithm. </p>

</code></pre>|null|https://s3.amazonaws.com/ghost-for-ash/2017/03/gauss.jpg|0|0|published|en_US|public|Adventures with Gauss and a partial pivot|An algorithm for Gaussian Elimination with Partial Pivot in Python and advice on how to stay sane in the face of nightmarish matrix indexes.|2|2017-03-15 23:17:44|2|2018-02-10 01:35:24|2|2017-03-16 00:25:20|2|7|5ec0578d-ac7b-4f5b-bbae-342316b8cf68|dots|dots|A student presented an interesting algebra problem, as seen below. The goal is to find an expression that represents the total number of dots for any n.

![](https://cloud.thosedevs.com/2017/03/dots-1.jpg)

####break it down

First, create a table. Write down the number of white and orange dots, when n is 1, 2, 3 and 4.

<table style=""width:100%"" border align=""center"">
  <tr>
    <th align=""center"">n</th>
    <th align=""center"">white dots</th> 
    <th align=""center"">orange dots</th>
  </tr>
  <tr>
    <td align=""center"">1</td>
    <td align=""center"">0</td>
    <td align=""center"">4</td>
  </tr>
  <tr>
    <td align=""center"">2</td>
    <td align=""center"">1</td>
    <td align=""center"">8</td>
  </tr>
  <tr>
    <td align=""center"">3</td>
    <td align=""center"">4</td>
    <td align=""center"">12</td>
  </tr>
  <tr>
    <td align=""center"">4</td>
    <td align=""center"">9</td>
    <td align=""center"">16</td>
  </tr>
</table>

Start with the easy stuff. Find a relationship between n and the number of orange dots.

<table style=""width:100%"" border align=""center"">
  <tr>
    <th align=""center"">n</th>
    <th align=""center"">orange dots</th> 
    <th align=""center"">math</th>
  </tr>
  <tr>
    <td align=""center"">1</td>
    <td align=""center"">4</td>
    <td align=""center"">1 x 4 = 4</td>
  </tr>
  <tr>
    <td align=""center"">2</td>
    <td align=""center"">8</td>
    <td align=""center"">2 x 4 = 8</td>
  </tr>
  <tr>
    <td align=""center"">3</td>
    <td align=""center"">12</td>
    <td align=""center"">3 x 4 = 12</td>
  </tr>
  <tr>
    <td align=""center"">4</td>
    <td align=""center"">16</td>
    <td align=""center"">4 x 4 = 16</td>
  </tr>
</table>

The number of orange dots is given by 4n. 

Find a relationship between n and the number of white dots. For now, only use n = 1 and n = 2 to find the relationship.

<table style=""width:100%"" border align=""center"">
  <tr>
    <th align=""center"">n</th>
    <th align=""center"">white dots</th> 
    <th align=""center"">math</th>
  </tr>
  <tr>
    <td align=""center"">1</td>
    <td align=""center"">0</td>
    <td align=""center"">1 - 1 = 0</td>
  </tr>
  <tr>
    <td align=""center"">2</td>
    <td align=""center"">1</td>
    <td align=""center"">2 - 1 = 1</td>
  </tr>
  <tr>
    <td align=""center"">3</td>
    <td align=""center"">4</td>
    <td align=""center"">3 - 1 = 2</td>
  </tr>
  <tr>
    <td align=""center"">4</td>
    <td align=""center"">9</td>
    <td align=""center"">4 - 1 = 3</td>
  </tr>
</table>

n - 1 can be used to find the number of white dots when n = 1 and when n = 2. However, it cannot be used when n = 3 and when n = 4. Expand on what we have so far, (n - 1), so that the expression holds for n = 3 and n = 4.

<table style=""width:100%"" border align=""center"">
  <tr>
    <th align=""center"">n</th>
    <th align=""center"">white dots</th> 
    <th align=""center"">math</th>
  </tr>
  <tr>
    <td align=""center"">1</td>
    <td align=""center"">0</td>
    <td align=""center"">1 - 1 = 0</td>
  </tr>
  <tr>
    <td align=""center"">2</td>
    <td align=""center"">1</td>
    <td align=""center"">2 - 1 = 1</td>
  </tr>
  <tr>
    <td align=""center"">3</td>
    <td align=""center"">4</td>
    <td align=""center"">3 - 1 = 2, and 2 x 2 = 4</td>
  </tr>
  <tr>
    <td align=""center"">4</td>
    <td align=""center"">9</td>
    <td align=""center"">4 - 1 = 3, and 3 x 3 = 9</td>
  </tr>
</table>

The number of white dots is given by (n-1)<sup>2</sup>.

The total number of dots is therefore given by (n-1)<sup>2</sup> + 4n. Distribute and simplify to get:

n<sup>2</sup> + 2n + 1.

####the big picture
The quadratic n<sup>2</sup> + 2n + 1 is a parabola. In algebra, students are inundated with quadratics like this one. They are asked to graph the quadratic, find the vertex, and find if the solutions are real or imaginary. They are taught several methods to solve or graph these types of problems (i.e. jargon like factoring, completing the square, quadratic formula, and vertex form). Algebra can quickly turn into drudge-work and we can lose sight of the bigger picture.

This assignment is refreshing because it reminds us that math, being the universal language, fundamentally enables us to describe our observations. Those observations may be a pattern of dots, a fired arrow, a thrown ball, or a satellite dish. We can appreciate that a quadratic is more than just a parabola.|null|<p>A student presented an interesting algebra problem, as seen below. The goal is to find an expression that represents the total number of dots for any n.</p>

<p><img src=""https://cloud.thosedevs.com/2017/03/dots-1.jpg"" alt="""" /></p>

<h4 id=""breakitdown"">break it down</h4>

<p>First, create a table. Write down the number of white and orange dots, when n is 1, 2, 3 and 4.</p>

<table style=""width:100%"" border align=""center"">  
  <tr>
    <th align=""center"">n</th>
    <th align=""center"">white dots</th> 
    <th align=""center"">orange dots</th>
  </tr>
  <tr>
    <td align=""center"">1</td>
    <td align=""center"">0</td>
    <td align=""center"">4</td>
  </tr>
  <tr>
    <td align=""center"">2</td>
    <td align=""center"">1</td>
    <td align=""center"">8</td>
  </tr>
  <tr>
    <td align=""center"">3</td>
    <td align=""center"">4</td>
    <td align=""center"">12</td>
  </tr>
  <tr>
    <td align=""center"">4</td>
    <td align=""center"">9</td>
    <td align=""center"">16</td>
  </tr>
</table>

<p>Start with the easy stuff. Find a relationship between n and the number of orange dots.</p>

<table style=""width:100%"" border align=""center"">  
  <tr>
    <th align=""center"">n</th>
    <th align=""center"">orange dots</th> 
    <th align=""center"">math</th>
  </tr>
  <tr>
    <td align=""center"">1</td>
    <td align=""center"">4</td>
    <td align=""center"">1 x 4 = 4</td>
  </tr>
  <tr>
    <td align=""center"">2</td>
    <td align=""center"">8</td>
    <td align=""center"">2 x 4 = 8</td>
  </tr>
  <tr>
    <td align=""center"">3</td>
    <td align=""center"">12</td>
    <td align=""center"">3 x 4 = 12</td>
  </tr>
  <tr>
    <td align=""center"">4</td>
    <td align=""center"">16</td>
    <td align=""center"">4 x 4 = 16</td>
  </tr>
</table>

<p>The number of orange dots is given by 4n. </p>

<p>Find a relationship between n and the number of white dots. For now, only use n = 1 and n = 2 to find the relationship.</p>

<table style=""width:100%"" border align=""center"">  
  <tr>
    <th align=""center"">n</th>
    <th align=""center"">white dots</th> 
    <th align=""center"">math</th>
  </tr>
  <tr>
    <td align=""center"">1</td>
    <td align=""center"">0</td>
    <td align=""center"">1 - 1 = 0</td>
  </tr>
  <tr>
    <td align=""center"">2</td>
    <td align=""center"">1</td>
    <td align=""center"">2 - 1 = 1</td>
  </tr>
  <tr>
    <td align=""center"">3</td>
    <td align=""center"">4</td>
    <td align=""center"">3 - 1 = 2</td>
  </tr>
  <tr>
    <td align=""center"">4</td>
    <td align=""center"">9</td>
    <td align=""center"">4 - 1 = 3</td>
  </tr>
</table>

<p>n - 1 can be used to find the number of white dots when n = 1 and when n = 2. However, it cannot be used when n = 3 and when n = 4. Expand on what we have so far, (n - 1), so that the expression holds for n = 3 and n = 4.</p>

<table style=""width:100%"" border align=""center"">  
  <tr>
    <th align=""center"">n</th>
    <th align=""center"">white dots</th> 
    <th align=""center"">math</th>
  </tr>
  <tr>
    <td align=""center"">1</td>
    <td align=""center"">0</td>
    <td align=""center"">1 - 1 = 0</td>
  </tr>
  <tr>
    <td align=""center"">2</td>
    <td align=""center"">1</td>
    <td align=""center"">2 - 1 = 1</td>
  </tr>
  <tr>
    <td align=""center"">3</td>
    <td align=""center"">4</td>
    <td align=""center"">3 - 1 = 2, and 2 x 2 = 4</td>
  </tr>
  <tr>
    <td align=""center"">4</td>
    <td align=""center"">9</td>
    <td align=""center"">4 - 1 = 3, and 3 x 3 = 9</td>
  </tr>
</table>

<p>The number of white dots is given by (n-1)<sup>2</sup>.</p>

<p>The total number of dots is therefore given by (n-1)<sup>2</sup> + 4n. Distribute and simplify to get:</p>

<p>n<sup>2</sup> + 2n + 1.</p>

<h4 id=""thebigpicture"">the big picture</h4>

<p>The quadratic n<sup>2</sup> + 2n + 1 is a parabola. In algebra, students are inundated with quadratics like this one. They are asked to graph the quadratic, find the vertex, and find if the solutions are real or imaginary. They are taught several methods to solve or graph these types of problems (i.e. jargon like factoring, completing the square, quadratic formula, and vertex form). Algebra can quickly turn into drudge-work and we can lose sight of the bigger picture.</p>

<p>This assignment is refreshing because it reminds us that math, being the universal language, fundamentally enables us to describe our observations. Those observations may be a pattern of dots, a fired arrow, a thrown ball, or a satellite dish. We can appreciate that a quadratic is more than just a parabola.</p>|null|https://cloud.thosedevs.com/2017/03/asheville.jpg|0|0|published|en_US|public|dots|Find an expression that represents the total number of dots for any n.|2|2017-03-17 13:25:18|2|2018-02-10 01:35:11|2|2017-03-17 14:31:01|2|9|55486396-2df4-4a84-bb9c-1e71c809e77b|Backpacking with Dad|backpacking-with-dad|I grew up in a 90s suburbia and I wanted for nothing materially. As I was the first child, my family doted on me. I was like Dudley, surrounded by birthday presents, although my parents were nothing like the Dursleys. However, I was spoiled and disconnected. I didn't care about school, or nature, or anything that was not a video game, my American Girl dolls, my presents, or my family and friends. I lived fully in the moment, I could not see the importance of things that did not immediately affect me.

Meanwhile, Dad and Mom were young sometimes struggling parents that made mistakes, triumphs, and in general were doing their best to provide for me.
######Dad saw the contrast between my life of ease and his reality and decided it was time for me to backpack.
<p></p>
This is our story; these are the hikes:
<ul></ul>
- [Water Tower](#WT)
- [Yosemite Falls](#YF)
- [Thousand Island Lakes](#TI)
- [Yosemite to Mammoth](#YM)
- [Duck Pass](#DP)
- [Mount Whitney](#MW)
- [Mammoth to Tuolumne](#MMTUO)
- [Mammoth](#MM)
- [Father's Day](#FD)

---

##<a name=""WT""></a>The Water Tower
The local water tower is where it all started. Dad used to take me and my short chubby legs up the hill to the water tower behind our house. Three of my strides equaled one of his and I could not get enough air! Dad made it look so easy, his thin body seemingly bounced up the hill with little effort. The round trip hike was about a mile and truly not that difficult, but I hated hiking up to that tower and I thought it was strenuous and I was truth be told afraid of the tower.

######I convinced myself that the gates guarding the tower protected citizens from velociraptors.
<p></p>

There were fun moments, however. This was the first spot my father ever took me stargazing. The park lights polluted the sky too much, so we couldn't see anything other than the moon and alien ships, I mean airplanes.

Now, I visit the water tower whenever I visit home because it is my sanctuary. It is surrounded by dry grasses, sweet smelling peppercorn trees, sage, rosemary, and gentle breezes. It is a reservoir of memories. 

---

##<a name=""YF""></a>Yosemite Falls
Tourists, hikers, and rock climbers come to Yosemite to view and to be awe-inspired by the massive and unforgiving, once magma, glacier-sculpted granite alive by thunderous falls and softened by dewy meadows.

Dad took Mom, Britt, and me to Yosemite during our summer vacation. While Mom watched over her nutty kids, Dad would venture off to do several day hikes, including Half Dome. He also risked taking me up the falls because he wanted to show me that I could accomplish something, if only it was a hike to the top of Yosemite Falls. Note that at this point in my life I was a straight D fourth grader with needless to say few accomplishments. I was happy to spend some father-daughter time.

We reached the falls easily and took a break to admire the accomplishment and the view. 

######""Do not walk over to look at the falls. That is how people die."" 
<p></p>
A few forgetful moments later, I walked over in aim to see what the top of a waterfall looked like. I almost got to the edge when I heard my dad scream, ""ASHLEY!"" Fearing the wrath of my dad, but not for my life as I failed to comprehend the gravity of the situation, I ran back to him. I nearly scared him to death.

---

##<a name=""TI""></a>Thousand Islands
Despite my near death experience, Dad was determined that I would not become completely detached from nature. I was 11 when I went on my first backpacking trip with dad.

I vividly remember our hotel beds before each camping trip. My Dad's blue pack would lay on one, with the tent, food, gear, poncho, flashlight, toothpaste and toothbrush, water filter, clothing, knives, bear can, sleeping bag, and maps spread around. 

######He would meticulously pack his wireframe with the tent at the bottom, proceeded by clothes, and resting on top all equipment we would need during a hike ordered by the likelihood that we would need a particular item. 
<p></p>
On the other bed was my green wireframe. At first, I attempted to shove everything into my pack in no order, as lazy children would, and so my Dad taught me the importance of thoughtful packing. Eventually, I was allowed to pack my own pack. 

We strolled 10 miles until our final decent into Thousand Island Lakes. My legs increasingly tired. I hated going uphill but learned then that going downhill was much worse for aching feet.

I remember looking down into Thousand Island Lakes and being blown away by the beauty. I had seen marvelous Yosemite and Sequoia. However, this kingdom was different because it was beautiful and it was away.

I was exhausted after hiking 10 miles and requested that we camp on the first beach we found. My Dad did not want to be so close to the main trail and so we painfully walked another mile when Dad happily picked out a camping spot by a glacier. 

I immediately set to work. I started carving a slide out of the glacier (it wasn't very long) while Dad rested. Slowly, I realized that I was becoming rather lumpy. Mosquitos were everywhere and decided to feast on me, the main course. I ran away screaming and looked over my shoulder at an angry hoard of big greedy-eyed mosquitos. 

After screaming and making a lot of noise running away, Dad convinced me to help him pitch the tent and make dinner. We brought stove tops, propane, coffee, hot chocolate, and those horrible little backpacker camping meals. I was too hungry to refuse. 

I went to bed, but not before attacking a mosquito that had recently fed. Disgusting.

The next morning felt like a hangover, not that I knew what one felt like back then. I was so cold, tired, grumpy, and hungry. I just wanted to go home. I started to get a bit of altitude sickness too.

On our hike back I remember mostly talking to my Dad about mosquitos. He told me useful bits of information like don't drink still water and consuming pink snow is not a fun way to die.

After our hike, Dad took me to get pizza or burgers or something that was unhealthy and greasy and therefore perfect. Civilization never felt so good. We went to Rite Aid. I bought anti-itch cream and counted the number of mosquito bites. I started with one arm. There were 20. I decided to stop counting. Dad took me to see Wild Wild West.   

---

##<a name=""YM""></a>Yosemite to Mammoth
Southern California was hot that August and the cool alpines were calling the wild.

Mammoth is a volcanic dome rising to 11,000 feet. Nearby are the Ansel Adams Wilderness named after the photographer. This special place is home to beautiful flowers, alpine trees, granite domes, deer, and clever black bears.

We stayed at Mammoth Lodge to acclimate and reduce the chance of altitude sickness before daring a 55-mile hike from Yosemite to Mammoth. We sat around watching young adventurous families mountain bike down Mammoth Mountain and frequented Yodler for their burgers.

While preparing for our hike, we decided to ditch the gourmet camp food and cookware. This was the start of our tradition, which if you ask me, is the only food one needs for backpacking trips less than a week. We loaded up on beef jerky, Powerade, Koolaid, water, trail mix, Starbursts, and Cliff or Powerbars bars. This combination provided enough liquids, sugar, protein, and electrolytes, to move lightly and quickly.
 
We caught an incredibly boring bus ride to Yosemite Valley. With much relief, the bus finished its 4-hour crawl, which would soon take us over four days. I was once again reminded of Yosemite's magnificence.

The worst part of any backpacking trip is waking up before dawn the first morning and leaving the comfort of a warm bed. It is dark at 3 am; we would hike by ambient light, starlight, moonlight, other people's flashlights, and if absolutely necessary we would pull out our flashlights too. I often emulated my dad in dress, sporting running shorts and a baggy sweatshirt. My spindly legs shivered as we ascended.

We crossed the paths of college students, who were backpacking along the John Muir Trail. I gazed at them in awe, as they were bravely in the wilderness without parents, which to me was a terrifying idea. They were delighted to see father and daughter hiking together. Their kindness and support fueled that day.

I did not see then what was so special about us however. Sure, I did not see a single other daughter hiking with her father, and I did not see that many children backpacking in the first place. To me, I was just along for the ride, and this was the time I got to spend with my dad. 

The college students split from the John Muir Trial to hike Half Dome. My dad refused when I asked if we could hike Half Dome too; something about not wanting to mess with the cables. I think he may have still been a little shaken up after I almost dived off the falls on our last trip to Yosemite.

We progressed to Tuolumne Meadows, and the hike was again saturated with mosquitos. Dad assured that we would be bitten by fewer bugs as we climbed into and above the alpines, and magical region of crisp air, deep blue lakes, dark green trees, and few people. I was getting stronger, as for much of the year prior I had started running with my Dad. 

We hiked quickly and spent the night halfway between Yosemite and Tuolumne, a bit past Sunrise Camp, with hours of daylight to spare. I helped Dad pick out a spot free from red ants to pitch the tent, and attempted to read Lord of the Rings. The hobbits were not doing it for me, so I opted to climb to the top of a granite rock instead. I just sat there for awhile, too tired to formulate a thought. It was at least a little meditative. As soon as the sun went down, I complained about the lack of pillows, Dad told me to shut it, and we slumbered.

The next morning I woke ready to take on the day, and happily free from altitude sickness. The day started off well, but I was a sorry mess by the end of it. I didn't like downhills before, but this day I hated them. We hiked 15 miles that second day into Tuolumne Meadows. Fortunately, we ran into the college kids who literally cheered as we walked into camp. I gave them a sheepish smile.

We spent a rest day in Tuolumne. We sent my mom a pack of dirty clothes and ate a burger. Dad repacked my bag so that instead of 20 - 25 lbs, it was only 15. Dad's was much heavier, but he slimmed down his pack as well. Dad and I then went into the local mountain convenience shop. My Dad duel wielded a beer and potato chips and he bought me an ice cream, which I happily ate while looking across the meadows at the distant Donohue pass. A father and son found their way to the store and stopped to chat. They had just come down from Donohue pass.

Donohue pass is an 11,000 foot pass, sitting about 12 miles from Tuolumne Meadows Camp ground. The pass itself climbs 2,000 feet.

""Where did you come in from?"" They asked.

""From the valley. We're headed up to the pass tomorrow. How is it up there?""

""It's difficult."" They sized my narrow frame and remarked, 

######""She won't make it.""
<p></p>

My eyes snapped up and they suffered a steely glare.

My father later told me, over too many Baileys in Aspen, that his favorite memory of me is when I glared at the two backpackers and the hike that followed. To him, my glare screamed, ""F**k you, man.""

If the beef jerky and endless supply of sugar was not enough fuel, that comment certainly was. We blazed through the meadow, I was too preoccupied and fiery to notice the beauty. Near the top of the pass, I had calmed down enough to notice a gorgeous lake and a strange man. I remember asking my dad what was wrong with him. He pretended to smoke a joint and said, ""dope"". I had no clue what that meant.

We got to the top of the pass, and I proved the father and son wrong. More importantly, I proved to myself that I could climb difficult passes.

Dad explained that camping above 10,000 feet is a good way to bring about altitude sickness. On our way down there was a raptor of a bird sitting on one of the switchback walls. Unfortunately, this meant we had to pass the bird twice. It eyed us greedily and prominently displayed its velociraptor talons.

We climbed down to 9,000 feet to settle in for the night. 2,000 up and 2,000 down. Ouch. That night, we fell asleep before the sun had set.

We continued our hike towards Mammoth Lakes. I mostly remember wishing I could be like Harry Potter, and ride a broomstick home. Interlaced with these wishful thoughts are brief memories of meadows, trees, lakes, and a peaceful silence.

We got to Mammoth and had another Yodler burger that tasted significantly better. My dad asked me if I wanted to rock climb. I looked at him like he was a crazy man. We ate at Grumpy's later, I played a lot of Pac-Man and indulged in their free popcorn. We regretted the meal later as both our stomachs were slightly upset with us afterward.

---

##<a name=""DP""></a>Duck Pass
My sister and I are about six years apart. After several years of torture in the Sierras; Dad decided to extend the love to my sister who was now old enough. Dad was trying to ease her into hiking, so the trip was shorter than what we were used to. She could have handled more, I call her ""Little Terror"" among other things because she's insane.

Little is athletically strong, mentally strong, stubborn, and a natural leader in various facets (she has been captain in sports teams, lead a 200 person guild in a popular video game, and now is an officer in the Navy). She is awesome. 

We hiked along the <a href=""http://www.mammothtrails.org/trail/9/duck-pass-trail/"" target=""_blank"">Duck Pass Trail</a>. I do not remember much of the short hike, other than Little's altitude sickness ending up on all of our sleeping bags in the middle of the night. The pristine lake helped to purify our sleeping bags the next day (sorry future hikers).

---

##<a name=""MW""></a>Mount Whitney
It was the summer before Softmore year of high school. The cross country team was gearing up for high altitude training at Mammoth and was to meet Deena Kastor. My Dad was preparing me for another adventure. 

Mount Whitney was one of those things where Dad said, ""We're hiking Mount Whitney this summer."" The mountain was not on my bucket list, but I didn't have much of a choice. 

Mount Whitney is the tallest mountain in the contiguous United States, sitting at 14,505 feet. Mount Whitney is easily seen from the 395, the highway Southern Californians take en route to Mammoth. However, tourists drive by the giant as from the highway perspective Mount Whitney blends in with the other dramatic faces in my Sierra Nevada playground.

This was going to be Dad's second trip up Mount Whitney. As a child, I heard stories of my Dad's first trip with Mom. They spoke about the vertical meadow, how they wanted to do nothing but read after the trip, and how a man had died on the mountain shortly after their summit.

Summer storms roll through the Sierras often, and they can be dangerous. Dad always warned about the dangers of being exposed during a Sierra afternoon storm. A man was caught in a typical storm at the top of Mount Whitney and decided he would ride out the storm in the summit's shelter. Lighting struck the metal roof, acting as a conductor to his metal glasses. Possibly because of this story, I never had a deep desire to climb Mount Whitney.

It was August and hot as usual. Dad and I drove to Mammoth Lakes to acclimate once again. I had missed the sweet summer air and cool mountain breezes. Unfortunately, we could not stay too long. We drove back into the desert and spent the night before our hike in Lone Pine. 

Lone Pine is not urban or rural, it's frontier. The town itself is sleepy and always seemed a little depressing to me, although the dramatic uplift of the Sierras is impressively beautiful if not intimidating.

We stayed at the Dow Villa Motel, I would rather camp on the cold hard ground in the alpines than settle for small comforts like a cheap mattress, a heavily chlorinated pool, and a TV. I suppose this marks where I truly started becoming outdoorsy. I stared continuously at Whitney's east face, and became more and more intrigued by the mysterious mountain. 

We woke before 3 am; I was tired, cold, hungry, but excited. We hiked by moonlight at first. I felt like I was back, back where I belonged, as my feet stepped onto the trail at Whitney Portal. We immediately hit switchbacks and I reveled in the feeling of slowly climbing, savored the smell of the fresh morning air, and listened to our footsteps and to the slowly awakening mountain. I was never afraid because I was with my dad. Not far into the hike, during a short break, my Dad said to me, 

######""I wanted to prove to you that you can do anything you set your mind to.""
<p></p>
We saw a few backpackers who were starting or finishing the John Muir Trail. We even saw one couple hiking the John Muir Trail as part of their honeymoon.

We ascended to what I felt was the famous vertical meadow. Plantlife laced a steep talus gradient, water trickled through. We soon after reached High Camp, a populated campground at 12,000 feet. I threw down my pack at our campsite and queried if we could stay the night then summit in the morning. Dad reminded that sleeping above 10,000 feet was little fun, and we would be grateful tomorrow for an easy hike down if we climbed to the top today. 

We rested a few hours before starting the 99 switchbacks.

My legs were feeling slightly more springy due to the long break, I opted to sing ""99 bottles of beer on the wall"", which irked various other hikers.

We arrived at the infamous Whitney windows. The windows were a portion of the trail with steep several hundred foot drops on either side. The trails were narrow enough that if you tumbled, there was a good chance of a murderous free fall. A few careless young adults skirted along the edge of one of windows to have their photo taken. My dad made me promise that I would never be that stupid. No problem.

The hardest part of the entire hike was the last 400 meters. The air was a little thinner and a little harder to breathe, and my anaerobic muscles burned. Dad taught me a technique to help get up the last portion; take a few long strides, rest, repeat until the crest.

In life, we collect moments. I know my happiest moment ever, my favorite moment of my dad, mom, and sister, the moment I regret the most, and the moment that was storybook perfect. This was my weirdest moment. I spent years looking up at the majestic Sierra Nevadas, and now I was looking down at all of it. It felt to me as if the world had inverted, it was strange. 

Our climb back down to base camp was tricky. We picked our way past the windows, deliriously descended the switchbacks, and fell asleep at High Camp.

Dad was right, the next morning was rough and sleeping below 10,000 feet is the preferable option. Fortunately, the hike down was easy, minus the battering of our feet and legs from the constant descent.

Back in Lone Pine, Dad bought me a purple shirt that said, ""I climbed Mount Whitney.""

I have a short bucket list now; one of those items is to hike the John Muir Trail and summit Mount Whitney with my sister.

---

##<a name=""MMTUO""></a>Mammoth to Tuolumne
Dad was looking to repeat the Yosemite to Mammoth hike with my sister, but instead going from Yosemite to Mammoth we planned on going from Mammoth to Yosemite and included a few High Sierra Camps.

We set out from Mammoth Lakes in good health and spirit. Here is one of our tent free campsites, from left to right featuring my sister's backpack, the later to be abused bear can, my green wireframe, my Dad's blue wireframe, and our neglected boots strewn about.

![test](https://cloud.thosedevs.com/2017/03/mm2tuocamp.jpg)

That night we slept under tree cover. Little shook me in the middle of the night.

######Little panicked, ""Ashy! There's a bear!""  
<p></p>
I whisper-yelled, ""Dad wake up! Bear!""

Dad said unphased, ""There's no bear. Go back to sleep.""

Little found it hard to sleep and continued to say, ""I really think there's a bear."" 

Dad said to me the next morning, chuckling a little, ""Don't tell your sister, I couldn't find the bear can at first. She was right, there was a bear. He whacked the can 200 yards downstream. The thing was strong!"" Years later, over dinner, she would learn the truth. 

We continued our journey, entered Merced Lake High Sierra Camp and rested a night.

![test](https://cloud.thosedevs.com/2017/03/HSC.jpg)

The next morning, we climbed from Merced Lake to Vogelsang, a very strenuous 8-mile hike and also my favorite part. The early morning sun hit the granite switchbacks so that they glowed.

![test](https://cloud.thosedevs.com/2017/03/earlyriser.jpg)

After the ascent, we reached an alpine meadow, a short and merciful reprieve. Streams slowly split and reconvene, to nourish a serene high altitude meadow that is embraced by granite domes.     

![Merced to Vogelsang](https://cloud.thosedevs.com/2017/03/dadandme.jpg)

Near Vogelsang, we encountered a professor from my college. We exchanged a few words; he mentioned it was his first time in the Sierras and he was fascinated by the granite. I enthused my fortune of backpacking once a year. Then, I stupidly remarked, ""The geography is really interesting.""

He coldly studied me, ""Geology, not geography.""

Dad rolled his eyes. Privately he reminded that what matters is that I was out here, accomplishing feats, and more importantly appreciating nature.

What I appreciated most: trees will grow wherever they please, like between rocks.

![Nature Finds A Way](https://cloud.thosedevs.com/2017/03/naturefindsaway.jpg)

>*“Life, Uh, Finds a Way”* - Dr. Ian Malcolm, 1993

Geography and geology aside, geometry is also interesting.

![Rest Stop](https://cloud.thosedevs.com/2017/03/circles.jpg)

We stopped in Vogelsang High Sierra Camp, where the water was safe to drink without filters (at that time), the meals were delicious and fresh off the stove, and the hot chocolate free and plentiful. 

Dad crashed. My sister and I abandoned him and participated in a stargazing event. We laid on the ground and gazed upwards, as a guide taught us about photographing the night sky. I remember only being dazzled.

Unfortunately, at this point both Dad and Little became ill. Dad sustained on Koolaid and Starbursts alone. The climb down to Tuolumne was painful for them. We took a planned rest day and decided to stop our journey.

We caught a bus back to Mammoth, which drove by Mono Lake. I was secretly glad we bailed on the trail. The sky reflected off the lake, rays of purple and pink saturated the view. It was one of the prettiest things I have ever seen.

---

##<a name=""MM""></a>Mammoth
I was 25 and had just started a new job in Southern California. My sister was back from college and on her summer vacation. The moment was ripe for another backpacking trip, around Mammoth Lakes.

I left work late one July afternoon to drive to Mammoth Lakes. For the first time, I was driving alone. I blasted the music, rolled down the windows, and let lose. I sped through knolls by Joshua Tree, so that car felt like a roller coaster. I reveled in the long overdue sensation of the hot desert on my right and the dramatic Sierra Nevadas scenery to my left. Day turned to night; the moon was bright and large but partially hidden in-between wispy clouds. White, gray, purple, and black swam and billowed, painting a scene of a western, rugged, yet beautiful night. 

Little and Dad enthusiastically hugged me when I arrived. Dad treated me to a few drinks and Little to a few sodas at the bar. We spent the night preparing for our trip and amused that Dad, who always packs so light, was stuffing a bottle of wine into his blue wireframe.

The next morning, we quickly set into our hiking legs, albeit a little rusty. It was July, and my first time hiking in early summer. The flowers were out (as were the bugs) and plentiful. We walked through fields of flowers, climbed passes, and took in the scenery. 

![LT and flowers](https://cloud.thosedevs.com/2017/03/julycamp7.jpg)

We arrived at our campsite, a small beach, with a few hours left in the day. Little and I swam in the lake while Dad lake-chilled the wine. The lake was nearly freezing, so we hopped out. Little convinced me to climb a large, mosquito infested, talus field. We literally crawled, always maintaining three points of contact with the field. Below find Little sitting at the top, while I lagged behind, and the Minarets in the far distance.

![LT and Talus](https://cloud.thosedevs.com/2017/03/julycamp1.jpg)

It was tough work, and a dumb idea, but the views were great. We could see the Minarets, Mammoth Mountain (See below, right corner) and even a chair lift at the top of the field. 

![Ghost Logos](https://cloud.thosedevs.com/2017/03/julycamp8.jpg)

We could of course also see the lake we swam in one hour prior, and Dad pacing around the beach.

![Ghost Logos](https://cloud.thosedevs.com/2017/03/julycamp4.jpg)

With effort and anxiety, we climbed down the talus field and ate our dinner of beef jerkey, trail mix, and Kooliad. Night set, it was a freezing 32 degrees, and we were once again sleeping without a tent. We could see a spiral of the milky way clearly. Dad slept like a baby but Little and I were shivering. The two of us shared a sleeping bag to keep warm and streamed Mean Girls II on my smartphone, which took our minds off of the bitter cold. We soon dozed to sleep.

The next morning was rough as we were too cold to move. The sun started to slowly creep towards us, warming some of the granite boulders by our campsite last. Like lizards, Little and I sat on the rocks for added warmth until Dad persuaded us to clean up the site. ""The sooner we get moving, the sooner we'll warm up.""

A few miles into the trail, Dad exclaimed he had accidentally left the celebratory wine chilling in the lake! Hopefully, someone finds it and enjoys that wine for us.

This was our last backpacking trip, but not our last adventure. 

---

##<a name=""FD""></a>Father's Day
Like most working Americans, Dad did not get that much vacation time. What time he did have, he chose to spend with his daughters to teach life lessons and values. He spent countless hours with me and my sister in the wild, where we were mostly nuisances, because he wanted to raise strong women who believed in themselves and who were connected to nature. That is the most selfless action I have personally witnessed, and I am so incredibly appreciative.

I am biased, my heart believes that my dad is the best one out there although logically I know this world is full of wonderful fathers. To all the fathers who believe in their sons and daughters, thank you. You make the world a better place. 

######For you, Dad, I am forever grateful.

![dad](https://cloud.thosedevs.com/2017/03/dad.jpg)
|null|<p>I grew up in a 90s suburbia and I wanted for nothing materially. As I was the first child, my family doted on me. I was like Dudley, surrounded by birthday presents, although my parents were nothing like the Dursleys. However, I was spoiled and disconnected. I didn't care about school, or nature, or anything that was not a video game, my American Girl dolls, my presents, or my family and friends. I lived fully in the moment, I could not see the importance of things that did not immediately affect me.</p>

<p>Meanwhile, Dad and Mom were young sometimes struggling parents that made mistakes, triumphs, and in general were doing their best to provide for me.  </p>

<h6 id=""dadsawthecontrastbetweenmylifeofeaseandhisrealityanddecideditwastimeformetobackpack"">Dad saw the contrast between my life of ease and his reality and decided it was time for me to backpack.</h6>

<p></p>  

<p>This is our story; these are the hikes:  </p>

<ul></ul>  

<ul>
<li><a href=""#WT"">Water Tower</a></li>
<li><a href=""#YF"">Yosemite Falls</a></li>
<li><a href=""#TI"">Thousand Island Lakes</a></li>
<li><a href=""#YM"">Yosemite to Mammoth</a></li>
<li><a href=""#DP"">Duck Pass</a></li>
<li><a href=""#MW"">Mount Whitney</a></li>
<li><a href=""#MMTUO"">Mammoth to Tuolumne</a></li>
<li><a href=""#MM"">Mammoth</a></li>
<li><a href=""#FD"">Father's Day</a></li>
</ul>

<hr />

<h2 id=""anamewtathewatertower""><a name=""WT""></a>The Water Tower</h2>

<p>The local water tower is where it all started. Dad used to take me and my short chubby legs up the hill to the water tower behind our house. Three of my strides equaled one of his and I could not get enough air! Dad made it look so easy, his thin body seemingly bounced up the hill with little effort. The round trip hike was about a mile and truly not that difficult, but I hated hiking up to that tower and I thought it was strenuous and I was truth be told afraid of the tower.</p>

<h6 id=""iconvincedmyselfthatthegatesguardingthetowerprotectedcitizensfromvelociraptors"">I convinced myself that the gates guarding the tower protected citizens from velociraptors.</h6>

<p></p>

<p>There were fun moments, however. This was the first spot my father ever took me stargazing. The park lights polluted the sky too much, so we couldn't see anything other than the moon and alien ships, I mean airplanes.</p>

<p>Now, I visit the water tower whenever I visit home because it is my sanctuary. It is surrounded by dry grasses, sweet smelling peppercorn trees, sage, rosemary, and gentle breezes. It is a reservoir of memories. </p>

<hr />

<h2 id=""anameyfayosemitefalls""><a name=""YF""></a>Yosemite Falls</h2>

<p>Tourists, hikers, and rock climbers come to Yosemite to view and to be awe-inspired by the massive and unforgiving, once magma, glacier-sculpted granite alive by thunderous falls and softened by dewy meadows.</p>

<p>Dad took Mom, Britt, and me to Yosemite during our summer vacation. While Mom watched over her nutty kids, Dad would venture off to do several day hikes, including Half Dome. He also risked taking me up the falls because he wanted to show me that I could accomplish something, if only it was a hike to the top of Yosemite Falls. Note that at this point in my life I was a straight D fourth grader with needless to say few accomplishments. I was happy to spend some father-daughter time.</p>

<p>We reached the falls easily and took a break to admire the accomplishment and the view. </p>

<h6 id=""donotwalkovertolookatthefallsthatishowpeopledie"">""Do not walk over to look at the falls. That is how people die.""</h6>

<p></p>  

<p>A few forgetful moments later, I walked over in aim to see what the top of a waterfall looked like. I almost got to the edge when I heard my dad scream, ""ASHLEY!"" Fearing the wrath of my dad, but not for my life as I failed to comprehend the gravity of the situation, I ran back to him. I nearly scared him to death.</p>

<hr />

<h2 id=""anametiathousandislands""><a name=""TI""></a>Thousand Islands</h2>

<p>Despite my near death experience, Dad was determined that I would not become completely detached from nature. I was 11 when I went on my first backpacking trip with dad.</p>

<p>I vividly remember our hotel beds before each camping trip. My Dad's blue pack would lay on one, with the tent, food, gear, poncho, flashlight, toothpaste and toothbrush, water filter, clothing, knives, bear can, sleeping bag, and maps spread around. </p>

<h6 id=""hewouldmeticulouslypackhiswireframewiththetentatthebottomproceededbyclothesandrestingontopallequipmentwewouldneedduringahikeorderedbythelikelihoodthatwewouldneedaparticularitem"">He would meticulously pack his wireframe with the tent at the bottom, proceeded by clothes, and resting on top all equipment we would need during a hike ordered by the likelihood that we would need a particular item.</h6>

<p></p>  

<p>On the other bed was my green wireframe. At first, I attempted to shove everything into my pack in no order, as lazy children would, and so my Dad taught me the importance of thoughtful packing. Eventually, I was allowed to pack my own pack. </p>

<p>We strolled 10 miles until our final decent into Thousand Island Lakes. My legs increasingly tired. I hated going uphill but learned then that going downhill was much worse for aching feet.</p>

<p>I remember looking down into Thousand Island Lakes and being blown away by the beauty. I had seen marvelous Yosemite and Sequoia. However, this kingdom was different because it was beautiful and it was away.</p>

<p>I was exhausted after hiking 10 miles and requested that we camp on the first beach we found. My Dad did not want to be so close to the main trail and so we painfully walked another mile when Dad happily picked out a camping spot by a glacier. </p>

<p>I immediately set to work. I started carving a slide out of the glacier (it wasn't very long) while Dad rested. Slowly, I realized that I was becoming rather lumpy. Mosquitos were everywhere and decided to feast on me, the main course. I ran away screaming and looked over my shoulder at an angry hoard of big greedy-eyed mosquitos. </p>

<p>After screaming and making a lot of noise running away, Dad convinced me to help him pitch the tent and make dinner. We brought stove tops, propane, coffee, hot chocolate, and those horrible little backpacker camping meals. I was too hungry to refuse. </p>

<p>I went to bed, but not before attacking a mosquito that had recently fed. Disgusting.</p>

<p>The next morning felt like a hangover, not that I knew what one felt like back then. I was so cold, tired, grumpy, and hungry. I just wanted to go home. I started to get a bit of altitude sickness too.</p>

<p>On our hike back I remember mostly talking to my Dad about mosquitos. He told me useful bits of information like don't drink still water and consuming pink snow is not a fun way to die.</p>

<p>After our hike, Dad took me to get pizza or burgers or something that was unhealthy and greasy and therefore perfect. Civilization never felt so good. We went to Rite Aid. I bought anti-itch cream and counted the number of mosquito bites. I started with one arm. There were 20. I decided to stop counting. Dad took me to see Wild Wild West.   </p>

<hr />

<h2 id=""anameymayosemitetomammoth""><a name=""YM""></a>Yosemite to Mammoth</h2>

<p>Southern California was hot that August and the cool alpines were calling the wild.</p>

<p>Mammoth is a volcanic dome rising to 11,000 feet. Nearby are the Ansel Adams Wilderness named after the photographer. This special place is home to beautiful flowers, alpine trees, granite domes, deer, and clever black bears.</p>

<p>We stayed at Mammoth Lodge to acclimate and reduce the chance of altitude sickness before daring a 55-mile hike from Yosemite to Mammoth. We sat around watching young adventurous families mountain bike down Mammoth Mountain and frequented Yodler for their burgers.</p>

<p>While preparing for our hike, we decided to ditch the gourmet camp food and cookware. This was the start of our tradition, which if you ask me, is the only food one needs for backpacking trips less than a week. We loaded up on beef jerky, Powerade, Koolaid, water, trail mix, Starbursts, and Cliff or Powerbars bars. This combination provided enough liquids, sugar, protein, and electrolytes, to move lightly and quickly.</p>

<p>We caught an incredibly boring bus ride to Yosemite Valley. With much relief, the bus finished its 4-hour crawl, which would soon take us over four days. I was once again reminded of Yosemite's magnificence.</p>

<p>The worst part of any backpacking trip is waking up before dawn the first morning and leaving the comfort of a warm bed. It is dark at 3 am; we would hike by ambient light, starlight, moonlight, other people's flashlights, and if absolutely necessary we would pull out our flashlights too. I often emulated my dad in dress, sporting running shorts and a baggy sweatshirt. My spindly legs shivered as we ascended.</p>

<p>We crossed the paths of college students, who were backpacking along the John Muir Trail. I gazed at them in awe, as they were bravely in the wilderness without parents, which to me was a terrifying idea. They were delighted to see father and daughter hiking together. Their kindness and support fueled that day.</p>

<p>I did not see then what was so special about us however. Sure, I did not see a single other daughter hiking with her father, and I did not see that many children backpacking in the first place. To me, I was just along for the ride, and this was the time I got to spend with my dad. </p>

<p>The college students split from the John Muir Trial to hike Half Dome. My dad refused when I asked if we could hike Half Dome too; something about not wanting to mess with the cables. I think he may have still been a little shaken up after I almost dived off the falls on our last trip to Yosemite.</p>

<p>We progressed to Tuolumne Meadows, and the hike was again saturated with mosquitos. Dad assured that we would be bitten by fewer bugs as we climbed into and above the alpines, and magical region of crisp air, deep blue lakes, dark green trees, and few people. I was getting stronger, as for much of the year prior I had started running with my Dad. </p>

<p>We hiked quickly and spent the night halfway between Yosemite and Tuolumne, a bit past Sunrise Camp, with hours of daylight to spare. I helped Dad pick out a spot free from red ants to pitch the tent, and attempted to read Lord of the Rings. The hobbits were not doing it for me, so I opted to climb to the top of a granite rock instead. I just sat there for awhile, too tired to formulate a thought. It was at least a little meditative. As soon as the sun went down, I complained about the lack of pillows, Dad told me to shut it, and we slumbered.</p>

<p>The next morning I woke ready to take on the day, and happily free from altitude sickness. The day started off well, but I was a sorry mess by the end of it. I didn't like downhills before, but this day I hated them. We hiked 15 miles that second day into Tuolumne Meadows. Fortunately, we ran into the college kids who literally cheered as we walked into camp. I gave them a sheepish smile.</p>

<p>We spent a rest day in Tuolumne. We sent my mom a pack of dirty clothes and ate a burger. Dad repacked my bag so that instead of 20 - 25 lbs, it was only 15. Dad's was much heavier, but he slimmed down his pack as well. Dad and I then went into the local mountain convenience shop. My Dad duel wielded a beer and potato chips and he bought me an ice cream, which I happily ate while looking across the meadows at the distant Donohue pass. A father and son found their way to the store and stopped to chat. They had just come down from Donohue pass.</p>

<p>Donohue pass is an 11,000 foot pass, sitting about 12 miles from Tuolumne Meadows Camp ground. The pass itself climbs 2,000 feet.</p>

<p>""Where did you come in from?"" They asked.</p>

<p>""From the valley. We're headed up to the pass tomorrow. How is it up there?""</p>

<p>""It's difficult."" They sized my narrow frame and remarked, </p>

<h6 id=""shewontmakeit"">""She won't make it.""</h6>

<p></p>

<p>My eyes snapped up and they suffered a steely glare.</p>

<p>My father later told me, over too many Baileys in Aspen, that his favorite memory of me is when I glared at the two backpackers and the hike that followed. To him, my glare screamed, ""F**k you, man.""</p>

<p>If the beef jerky and endless supply of sugar was not enough fuel, that comment certainly was. We blazed through the meadow, I was too preoccupied and fiery to notice the beauty. Near the top of the pass, I had calmed down enough to notice a gorgeous lake and a strange man. I remember asking my dad what was wrong with him. He pretended to smoke a joint and said, ""dope"". I had no clue what that meant.</p>

<p>We got to the top of the pass, and I proved the father and son wrong. More importantly, I proved to myself that I could climb difficult passes.</p>

<p>Dad explained that camping above 10,000 feet is a good way to bring about altitude sickness. On our way down there was a raptor of a bird sitting on one of the switchback walls. Unfortunately, this meant we had to pass the bird twice. It eyed us greedily and prominently displayed its velociraptor talons.</p>

<p>We climbed down to 9,000 feet to settle in for the night. 2,000 up and 2,000 down. Ouch. That night, we fell asleep before the sun had set.</p>

<p>We continued our hike towards Mammoth Lakes. I mostly remember wishing I could be like Harry Potter, and ride a broomstick home. Interlaced with these wishful thoughts are brief memories of meadows, trees, lakes, and a peaceful silence.</p>

<p>We got to Mammoth and had another Yodler burger that tasted significantly better. My dad asked me if I wanted to rock climb. I looked at him like he was a crazy man. We ate at Grumpy's later, I played a lot of Pac-Man and indulged in their free popcorn. We regretted the meal later as both our stomachs were slightly upset with us afterward.</p>

<hr />

<h2 id=""anamedpaduckpass""><a name=""DP""></a>Duck Pass</h2>

<p>My sister and I are about six years apart. After several years of torture in the Sierras; Dad decided to extend the love to my sister who was now old enough. Dad was trying to ease her into hiking, so the trip was shorter than what we were used to. She could have handled more, I call her ""Little Terror"" among other things because she's insane.</p>

<p>Little is athletically strong, mentally strong, stubborn, and a natural leader in various facets (she has been captain in sports teams, lead a 200 person guild in a popular video game, and now is an officer in the Navy). She is awesome. </p>

<p>We hiked along the <a href=""http://www.mammothtrails.org/trail/9/duck-pass-trail/"" target=""_blank"">Duck Pass Trail</a>. I do not remember much of the short hike, other than Little's altitude sickness ending up on all of our sleeping bags in the middle of the night. The pristine lake helped to purify our sleeping bags the next day (sorry future hikers).</p>

<hr />

<h2 id=""anamemwamountwhitney""><a name=""MW""></a>Mount Whitney</h2>

<p>It was the summer before Softmore year of high school. The cross country team was gearing up for high altitude training at Mammoth and was to meet Deena Kastor. My Dad was preparing me for another adventure. </p>

<p>Mount Whitney was one of those things where Dad said, ""We're hiking Mount Whitney this summer."" The mountain was not on my bucket list, but I didn't have much of a choice. </p>

<p>Mount Whitney is the tallest mountain in the contiguous United States, sitting at 14,505 feet. Mount Whitney is easily seen from the 395, the highway Southern Californians take en route to Mammoth. However, tourists drive by the giant as from the highway perspective Mount Whitney blends in with the other dramatic faces in my Sierra Nevada playground.</p>

<p>This was going to be Dad's second trip up Mount Whitney. As a child, I heard stories of my Dad's first trip with Mom. They spoke about the vertical meadow, how they wanted to do nothing but read after the trip, and how a man had died on the mountain shortly after their summit.</p>

<p>Summer storms roll through the Sierras often, and they can be dangerous. Dad always warned about the dangers of being exposed during a Sierra afternoon storm. A man was caught in a typical storm at the top of Mount Whitney and decided he would ride out the storm in the summit's shelter. Lighting struck the metal roof, acting as a conductor to his metal glasses. Possibly because of this story, I never had a deep desire to climb Mount Whitney.</p>

<p>It was August and hot as usual. Dad and I drove to Mammoth Lakes to acclimate once again. I had missed the sweet summer air and cool mountain breezes. Unfortunately, we could not stay too long. We drove back into the desert and spent the night before our hike in Lone Pine. </p>

<p>Lone Pine is not urban or rural, it's frontier. The town itself is sleepy and always seemed a little depressing to me, although the dramatic uplift of the Sierras is impressively beautiful if not intimidating.</p>

<p>We stayed at the Dow Villa Motel, I would rather camp on the cold hard ground in the alpines than settle for small comforts like a cheap mattress, a heavily chlorinated pool, and a TV. I suppose this marks where I truly started becoming outdoorsy. I stared continuously at Whitney's east face, and became more and more intrigued by the mysterious mountain. </p>

<p>We woke before 3 am; I was tired, cold, hungry, but excited. We hiked by moonlight at first. I felt like I was back, back where I belonged, as my feet stepped onto the trail at Whitney Portal. We immediately hit switchbacks and I reveled in the feeling of slowly climbing, savored the smell of the fresh morning air, and listened to our footsteps and to the slowly awakening mountain. I was never afraid because I was with my dad. Not far into the hike, during a short break, my Dad said to me, </p>

<h6 id=""iwantedtoprovetoyouthatyoucandoanythingyousetyourmindto"">""I wanted to prove to you that you can do anything you set your mind to.""</h6>

<p></p>  

<p>We saw a few backpackers who were starting or finishing the John Muir Trail. We even saw one couple hiking the John Muir Trail as part of their honeymoon.</p>

<p>We ascended to what I felt was the famous vertical meadow. Plantlife laced a steep talus gradient, water trickled through. We soon after reached High Camp, a populated campground at 12,000 feet. I threw down my pack at our campsite and queried if we could stay the night then summit in the morning. Dad reminded that sleeping above 10,000 feet was little fun, and we would be grateful tomorrow for an easy hike down if we climbed to the top today. </p>

<p>We rested a few hours before starting the 99 switchbacks.</p>

<p>My legs were feeling slightly more springy due to the long break, I opted to sing ""99 bottles of beer on the wall"", which irked various other hikers.</p>

<p>We arrived at the infamous Whitney windows. The windows were a portion of the trail with steep several hundred foot drops on either side. The trails were narrow enough that if you tumbled, there was a good chance of a murderous free fall. A few careless young adults skirted along the edge of one of windows to have their photo taken. My dad made me promise that I would never be that stupid. No problem.</p>

<p>The hardest part of the entire hike was the last 400 meters. The air was a little thinner and a little harder to breathe, and my anaerobic muscles burned. Dad taught me a technique to help get up the last portion; take a few long strides, rest, repeat until the crest.</p>

<p>In life, we collect moments. I know my happiest moment ever, my favorite moment of my dad, mom, and sister, the moment I regret the most, and the moment that was storybook perfect. This was my weirdest moment. I spent years looking up at the majestic Sierra Nevadas, and now I was looking down at all of it. It felt to me as if the world had inverted, it was strange. </p>

<p>Our climb back down to base camp was tricky. We picked our way past the windows, deliriously descended the switchbacks, and fell asleep at High Camp.</p>

<p>Dad was right, the next morning was rough and sleeping below 10,000 feet is the preferable option. Fortunately, the hike down was easy, minus the battering of our feet and legs from the constant descent.</p>

<p>Back in Lone Pine, Dad bought me a purple shirt that said, ""I climbed Mount Whitney.""</p>

<p>I have a short bucket list now; one of those items is to hike the John Muir Trail and summit Mount Whitney with my sister.</p>

<hr />

<h2 id=""anamemmtuoamammothtotuolumne""><a name=""MMTUO""></a>Mammoth to Tuolumne</h2>

<p>Dad was looking to repeat the Yosemite to Mammoth hike with my sister, but instead going from Yosemite to Mammoth we planned on going from Mammoth to Yosemite and included a few High Sierra Camps.</p>

<p>We set out from Mammoth Lakes in good health and spirit. Here is one of our tent free campsites, from left to right featuring my sister's backpack, the later to be abused bear can, my green wireframe, my Dad's blue wireframe, and our neglected boots strewn about.</p>

<p><img src=""https://cloud.thosedevs.com/2017/03/mm2tuocamp.jpg"" alt=""test"" /></p>

<p>That night we slept under tree cover. Little shook me in the middle of the night.</p>

<h6 id=""littlepanickedashytheresabear"">Little panicked, ""Ashy! There's a bear!""</h6>

<p></p>  

<p>I whisper-yelled, ""Dad wake up! Bear!""</p>

<p>Dad said unphased, ""There's no bear. Go back to sleep.""</p>

<p>Little found it hard to sleep and continued to say, ""I really think there's a bear."" </p>

<p>Dad said to me the next morning, chuckling a little, ""Don't tell your sister, I couldn't find the bear can at first. She was right, there was a bear. He whacked the can 200 yards downstream. The thing was strong!"" Years later, over dinner, she would learn the truth. </p>

<p>We continued our journey, entered Merced Lake High Sierra Camp and rested a night.</p>

<p><img src=""https://cloud.thosedevs.com/2017/03/HSC.jpg"" alt=""test"" /></p>

<p>The next morning, we climbed from Merced Lake to Vogelsang, a very strenuous 8-mile hike and also my favorite part. The early morning sun hit the granite switchbacks so that they glowed.</p>

<p><img src=""https://cloud.thosedevs.com/2017/03/earlyriser.jpg"" alt=""test"" /></p>

<p>After the ascent, we reached an alpine meadow, a short and merciful reprieve. Streams slowly split and reconvene, to nourish a serene high altitude meadow that is embraced by granite domes.     </p>

<p><img src=""https://cloud.thosedevs.com/2017/03/dadandme.jpg"" alt=""Merced to Vogelsang"" /></p>

<p>Near Vogelsang, we encountered a professor from my college. We exchanged a few words; he mentioned it was his first time in the Sierras and he was fascinated by the granite. I enthused my fortune of backpacking once a year. Then, I stupidly remarked, ""The geography is really interesting.""</p>

<p>He coldly studied me, ""Geology, not geography.""</p>

<p>Dad rolled his eyes. Privately he reminded that what matters is that I was out here, accomplishing feats, and more importantly appreciating nature.</p>

<p>What I appreciated most: trees will grow wherever they please, like between rocks.</p>

<p><img src=""https://cloud.thosedevs.com/2017/03/naturefindsaway.jpg"" alt=""Nature Finds A Way"" /></p>

<blockquote>
  <p><em>“Life, Uh, Finds a Way”</em> - Dr. Ian Malcolm, 1993</p>
</blockquote>

<p>Geography and geology aside, geometry is also interesting.</p>

<p><img src=""https://cloud.thosedevs.com/2017/03/circles.jpg"" alt=""Rest Stop"" /></p>

<p>We stopped in Vogelsang High Sierra Camp, where the water was safe to drink without filters (at that time), the meals were delicious and fresh off the stove, and the hot chocolate free and plentiful. </p>

<p>Dad crashed. My sister and I abandoned him and participated in a stargazing event. We laid on the ground and gazed upwards, as a guide taught us about photographing the night sky. I remember only being dazzled.</p>

<p>Unfortunately, at this point both Dad and Little became ill. Dad sustained on Koolaid and Starbursts alone. The climb down to Tuolumne was painful for them. We took a planned rest day and decided to stop our journey.</p>

<p>We caught a bus back to Mammoth, which drove by Mono Lake. I was secretly glad we bailed on the trail. The sky reflected off the lake, rays of purple and pink saturated the view. It was one of the prettiest things I have ever seen.</p>

<hr />

<h2 id=""anamemmamammoth""><a name=""MM""></a>Mammoth</h2>

<p>I was 25 and had just started a new job in Southern California. My sister was back from college and on her summer vacation. The moment was ripe for another backpacking trip, around Mammoth Lakes.</p>

<p>I left work late one July afternoon to drive to Mammoth Lakes. For the first time, I was driving alone. I blasted the music, rolled down the windows, and let lose. I sped through knolls by Joshua Tree, so that car felt like a roller coaster. I reveled in the long overdue sensation of the hot desert on my right and the dramatic Sierra Nevadas scenery to my left. Day turned to night; the moon was bright and large but partially hidden in-between wispy clouds. White, gray, purple, and black swam and billowed, painting a scene of a western, rugged, yet beautiful night. </p>

<p>Little and Dad enthusiastically hugged me when I arrived. Dad treated me to a few drinks and Little to a few sodas at the bar. We spent the night preparing for our trip and amused that Dad, who always packs so light, was stuffing a bottle of wine into his blue wireframe.</p>

<p>The next morning, we quickly set into our hiking legs, albeit a little rusty. It was July, and my first time hiking in early summer. The flowers were out (as were the bugs) and plentiful. We walked through fields of flowers, climbed passes, and took in the scenery. </p>

<p><img src=""https://cloud.thosedevs.com/2017/03/julycamp7.jpg"" alt=""LT and flowers"" /></p>

<p>We arrived at our campsite, a small beach, with a few hours left in the day. Little and I swam in the lake while Dad lake-chilled the wine. The lake was nearly freezing, so we hopped out. Little convinced me to climb a large, mosquito infested, talus field. We literally crawled, always maintaining three points of contact with the field. Below find Little sitting at the top, while I lagged behind, and the Minarets in the far distance.</p>

<p><img src=""https://cloud.thosedevs.com/2017/03/julycamp1.jpg"" alt=""LT and Talus"" /></p>

<p>It was tough work, and a dumb idea, but the views were great. We could see the Minarets, Mammoth Mountain (See below, right corner) and even a chair lift at the top of the field. </p>

<p><img src=""https://cloud.thosedevs.com/2017/03/julycamp8.jpg"" alt=""Ghost Logos"" /></p>

<p>We could of course also see the lake we swam in one hour prior, and Dad pacing around the beach.</p>

<p><img src=""https://cloud.thosedevs.com/2017/03/julycamp4.jpg"" alt=""Ghost Logos"" /></p>

<p>With effort and anxiety, we climbed down the talus field and ate our dinner of beef jerkey, trail mix, and Kooliad. Night set, it was a freezing 32 degrees, and we were once again sleeping without a tent. We could see a spiral of the milky way clearly. Dad slept like a baby but Little and I were shivering. The two of us shared a sleeping bag to keep warm and streamed Mean Girls II on my smartphone, which took our minds off of the bitter cold. We soon dozed to sleep.</p>

<p>The next morning was rough as we were too cold to move. The sun started to slowly creep towards us, warming some of the granite boulders by our campsite last. Like lizards, Little and I sat on the rocks for added warmth until Dad persuaded us to clean up the site. ""The sooner we get moving, the sooner we'll warm up.""</p>

<p>A few miles into the trail, Dad exclaimed he had accidentally left the celebratory wine chilling in the lake! Hopefully, someone finds it and enjoys that wine for us.</p>

<p>This was our last backpacking trip, but not our last adventure. </p>

<hr />

<h2 id=""anamefdafathersday""><a name=""FD""></a>Father's Day</h2>

<p>Like most working Americans, Dad did not get that much vacation time. What time he did have, he chose to spend with his daughters to teach life lessons and values. He spent countless hours with me and my sister in the wild, where we were mostly nuisances, because he wanted to raise strong women who believed in themselves and who were connected to nature. That is the most selfless action I have personally witnessed, and I am so incredibly appreciative.</p>

<p>I am biased, my heart believes that my dad is the best one out there although logically I know this world is full of wonderful fathers. To all the fathers who believe in their sons and daughters, thank you. You make the world a better place. </p>

<h6 id=""foryoudadiamforevergrateful"">For you, Dad, I am forever grateful.</h6>
