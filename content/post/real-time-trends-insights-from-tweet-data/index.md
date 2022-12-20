---
title: Real-time Trends Insights from Tweet Data
date: 2022-10-06T08:35:01.137Z
draft: false
featured: false
image:
  filename: featured
  focal_point: Smart
  preview_only: false
---
## 1. Local and global thought patterns
<p>While we might not be Twitter fans, we have to admit that it has a huge influence on the world (who doesn't know about Trump's tweets). Twitter data is not only gold in terms of insights, but <strong><em>Twitter-storms are available for analysis in near real-time</em></strong>. This means we can learn about the big waves of thoughts and moods around the world as they arise. </p>
<p>As any place filled with riches, Twitter has <em>security guards</em> blocking us from laying our hands on the data right away ⛔️ Some  authentication steps (really straightforward) are needed to call their APIs for data collection. Since our goal today is learning to extract insights from data, we have already gotten a green-pass from security ✅ Our data is ready for usage in the datasets folder — we can concentrate on the fun part! 🕵️‍♀️🌎
<br>
<br>
<img src="https://assets.datacamp.com/production/project_760/img/tweets_influence.png" style="width: 300px">
<hr>
<br>Twitter provides both global and local trends. Let's load and inspect data for topics that were hot worldwide (WW) and in the United States (US) at the moment of query  — snapshot of JSON response from the call to Twitter's <i>GET trends/place</i> API.</p>
<p><i><b>Note</b>: <a href="https://developer.twitter.com/en/docs/trends/trends-for-location/api-reference/get-trends-place.html">Here</a> is the documentation for this call, and <a href="https://developer.twitter.com/en/docs/api-reference-index.html">here</a> a full overview on Twitter's APIs.</i></p>


```python
# Loading json module
import json

# Load WW_trends and US_trends data into the the given variables respectively
WW_trends = json.loads(open('datasets/WWTrends.json').read())
US_trends = json.loads(open('datasets/USTrends.json').read())

# Inspecting data by printing out WW_trends and US_trends variables
print(WW_trends)
print(US_trends)
```

    [{'trends': [{'name': '#BeratKandili', 'url': 'http://twitter.com/search?q=%23BeratKandili', 'promoted_content': None, 'query': '%23BeratKandili', 'tweet_volume': 46373}, {'name': '#GoodFriday', 'url': 'http://twitter.com/search?q=%23GoodFriday', 'promoted_content': None, 'query': '%23GoodFriday', 'tweet_volume': 81891}, {'name': '#WeLoveTheEarth', 'url': 'http://twitter.com/search?q=%23WeLoveTheEarth', 'promoted_content': None, 'query': '%23WeLoveTheEarth', 'tweet_volume': 159698}, {'name': '#195TLdenTTVerilir', 'url': 'http://twitter.com/search?q=%23195TLdenTTVerilir', 'promoted_content': None, 'query': '%23195TLdenTTVerilir', 'tweet_volume': None}, {'name': '#AFLNorthDons', 'url': 'http://twitter.com/search?q=%23AFLNorthDons', 'promoted_content': None, 'query': '%23AFLNorthDons', 'tweet_volume': None}, {'name': 'Shiv Sena', 'url': 'http://twitter.com/search?q=%22Shiv+Sena%22', 'promoted_content': None, 'query': '%22Shiv+Sena%22', 'tweet_volume': None}, {'name': 'Lyra McKee', 'url': 'http://twitter.com/search?q=%22Lyra+McKee%22', 'promoted_content': None, 'query': '%22Lyra+McKee%22', 'tweet_volume': 17606}, {'name': 'Priyanka Chaturvedi', 'url': 'http://twitter.com/search?q=%22Priyanka+Chaturvedi%22', 'promoted_content': None, 'query': '%22Priyanka+Chaturvedi%22', 'tweet_volume': 22342}, {'name': 'Derry', 'url': 'http://twitter.com/search?q=Derry', 'promoted_content': None, 'query': 'Derry', 'tweet_volume': 28234}, {'name': '池袋の事故', 'url': 'http://twitter.com/search?q=%E6%B1%A0%E8%A2%8B%E3%81%AE%E4%BA%8B%E6%95%85', 'promoted_content': None, 'query': '%E6%B1%A0%E8%A2%8B%E3%81%AE%E4%BA%8B%E6%95%85', 'tweet_volume': 34381}, {'name': 'プリウス', 'url': 'http://twitter.com/search?q=%E3%83%97%E3%83%AA%E3%82%A6%E3%82%B9', 'promoted_content': None, 'query': '%E3%83%97%E3%83%AA%E3%82%A6%E3%82%B9', 'tweet_volume': 22944}, {'name': 'Hemant Karkare', 'url': 'http://twitter.com/search?q=%22Hemant+Karkare%22', 'promoted_content': None, 'query': '%22Hemant+Karkare%22', 'tweet_volume': 24067}, {'name': '高齢者', 'url': 'http://twitter.com/search?q=%E9%AB%98%E9%BD%A2%E8%80%85', 'promoted_content': None, 'query': '%E9%AB%98%E9%BD%A2%E8%80%85', 'tweet_volume': 28382}, {'name': '브이알', 'url': 'http://twitter.com/search?q=%EB%B8%8C%EC%9D%B4%EC%95%8C', 'promoted_content': None, 'query': '%EB%B8%8C%EC%9D%B4%EC%95%8C', 'tweet_volume': 15490}, {'name': '刀ステ', 'url': 'http://twitter.com/search?q=%E5%88%80%E3%82%B9%E3%83%86', 'promoted_content': None, 'query': '%E5%88%80%E3%82%B9%E3%83%86', 'tweet_volume': None}, {'name': '免許返納', 'url': 'http://twitter.com/search?q=%E5%85%8D%E8%A8%B1%E8%BF%94%E7%B4%8D', 'promoted_content': None, 'query': '%E5%85%8D%E8%A8%B1%E8%BF%94%E7%B4%8D', 'tweet_volume': None}, {'name': 'Berat Kandilimiz', 'url': 'http://twitter.com/search?q=%22Berat+Kandilimiz%22', 'promoted_content': None, 'query': '%22Berat+Kandilimiz%22', 'tweet_volume': 10901}, {'name': 'örgütdeğil arkadaşgrubu', 'url': 'http://twitter.com/search?q=%22%C3%B6rg%C3%BCtde%C4%9Fil+arkada%C5%9Fgrubu%22', 'promoted_content': None, 'query': '%22%C3%B6rg%C3%BCtde%C4%9Fil+arkada%C5%9Fgrubu%22', 'tweet_volume': None}, {'name': 'グレア', 'url': 'http://twitter.com/search?q=%E3%82%B0%E3%83%AC%E3%82%A2', 'promoted_content': None, 'query': '%E3%82%B0%E3%83%AC%E3%82%A2', 'tweet_volume': 23485}, {'name': '東京・池袋衝突事故', 'url': 'http://twitter.com/search?q=%E6%9D%B1%E4%BA%AC%E3%83%BB%E6%B1%A0%E8%A2%8B%E8%A1%9D%E7%AA%81%E4%BA%8B%E6%95%85', 'promoted_content': None, 'query': '%E6%9D%B1%E4%BA%AC%E3%83%BB%E6%B1%A0%E8%A2%8B%E8%A1%9D%E7%AA%81%E4%BA%8B%E6%95%85', 'tweet_volume': None}, {'name': '重体の女性と女児', 'url': 'http://twitter.com/search?q=%E9%87%8D%E4%BD%93%E3%81%AE%E5%A5%B3%E6%80%A7%E3%81%A8%E5%A5%B3%E5%85%90', 'promoted_content': None, 'query': '%E9%87%8D%E4%BD%93%E3%81%AE%E5%A5%B3%E6%80%A7%E3%81%A8%E5%A5%B3%E5%85%90', 'tweet_volume': None}, {'name': 'Lil Dicky', 'url': 'http://twitter.com/search?q=%22Lil+Dicky%22', 'promoted_content': None, 'query': '%22Lil+Dicky%22', 'tweet_volume': 42461}, {'name': '歩行者', 'url': 'http://twitter.com/search?q=%E6%AD%A9%E8%A1%8C%E8%80%85', 'promoted_content': None, 'query': '%E6%AD%A9%E8%A1%8C%E8%80%85', 'tweet_volume': 25405}, {'name': 'Derrick White', 'url': 'http://twitter.com/search?q=%22Derrick+White%22', 'promoted_content': None, 'query': '%22Derrick+White%22', 'tweet_volume': 27104}, {'name': '十二国記', 'url': 'http://twitter.com/search?q=%E5%8D%81%E4%BA%8C%E5%9B%BD%E8%A8%98', 'promoted_content': None, 'query': '%E5%8D%81%E4%BA%8C%E5%9B%BD%E8%A8%98', 'tweet_volume': 46803}, {'name': '#KpuJanganCurang', 'url': 'http://twitter.com/search?q=%23KpuJanganCurang', 'promoted_content': None, 'query': '%23KpuJanganCurang', 'tweet_volume': 75384}, {'name': '#HayırlıCumalar', 'url': 'http://twitter.com/search?q=%23Hay%C4%B1rl%C4%B1Cumalar', 'promoted_content': None, 'query': '%23Hay%C4%B1rl%C4%B1Cumalar', 'tweet_volume': 19848}, {'name': '#HayırlıKandiller', 'url': 'http://twitter.com/search?q=%23Hay%C4%B1rl%C4%B1Kandiller', 'promoted_content': None, 'query': '%23Hay%C4%B1rl%C4%B1Kandiller', 'tweet_volume': None}, {'name': '#HanumanJayanti', 'url': 'http://twitter.com/search?q=%23HanumanJayanti', 'promoted_content': None, 'query': '%23HanumanJayanti', 'tweet_volume': 83138}, {'name': '#IndonesianElectionHeroes', 'url': 'http://twitter.com/search?q=%23IndonesianElectionHeroes', 'promoted_content': None, 'query': '%23IndonesianElectionHeroes', 'tweet_volume': 19664}, {'name': '#يوم_الجمعه', 'url': 'http://twitter.com/search?q=%23%D9%8A%D9%88%D9%85_%D8%A7%D9%84%D8%AC%D9%85%D8%B9%D9%87', 'promoted_content': None, 'query': '%23%D9%8A%D9%88%D9%85_%D8%A7%D9%84%D8%AC%D9%85%D8%B9%D9%87', 'tweet_volume': 80799}, {'name': '#NRLBulldogsSouths', 'url': 'http://twitter.com/search?q=%23NRLBulldogsSouths', 'promoted_content': None, 'query': '%23NRLBulldogsSouths', 'tweet_volume': None}, {'name': '#NikahUmurBerapa', 'url': 'http://twitter.com/search?q=%23NikahUmurBerapa', 'promoted_content': None, 'query': '%23NikahUmurBerapa', 'tweet_volume': None}, {'name': '#DragRace', 'url': 'http://twitter.com/search?q=%23DragRace', 'promoted_content': None, 'query': '%23DragRace', 'tweet_volume': 37166}, {'name': '#ViernesSanto', 'url': 'http://twitter.com/search?q=%23ViernesSanto', 'promoted_content': None, 'query': '%23ViernesSanto', 'tweet_volume': None}, {'name': '#HardikPatel', 'url': 'http://twitter.com/search?q=%23HardikPatel', 'promoted_content': None, 'query': '%23HardikPatel', 'tweet_volume': None}, {'name': '#BLACKPINKxCorden', 'url': 'http://twitter.com/search?q=%23BLACKPINKxCorden', 'promoted_content': None, 'query': '%23BLACKPINKxCorden', 'tweet_volume': 253605}, {'name': '#Ontas', 'url': 'http://twitter.com/search?q=%23Ontas', 'promoted_content': None, 'query': '%23Ontas', 'tweet_volume': 27924}, {'name': '#ConCalmaRemix', 'url': 'http://twitter.com/search?q=%23ConCalmaRemix', 'promoted_content': None, 'query': '%23ConCalmaRemix', 'tweet_volume': 37846}, {'name': '#ProtestoEdiyorum', 'url': 'http://twitter.com/search?q=%23ProtestoEdiyorum', 'promoted_content': None, 'query': '%23ProtestoEdiyorum', 'tweet_volume': None}, {'name': '#DinahJane1', 'url': 'http://twitter.com/search?q=%23DinahJane1', 'promoted_content': None, 'query': '%23DinahJane1', 'tweet_volume': 23757}, {'name': '#ShivSena', 'url': 'http://twitter.com/search?q=%23ShivSena', 'promoted_content': None, 'query': '%23ShivSena', 'tweet_volume': None}, {'name': '#DuyguAsena', 'url': 'http://twitter.com/search?q=%23DuyguAsena', 'promoted_content': None, 'query': '%23DuyguAsena', 'tweet_volume': None}, {'name': '#TheJudasInMyLife', 'url': 'http://twitter.com/search?q=%23TheJudasInMyLife', 'promoted_content': None, 'query': '%23TheJudasInMyLife', 'tweet_volume': None}, {'name': '#Jersey', 'url': 'http://twitter.com/search?q=%23Jersey', 'promoted_content': None, 'query': '%23Jersey', 'tweet_volume': 20509}, {'name': '#اغلاق_BBM', 'url': 'http://twitter.com/search?q=%23%D8%A7%D8%BA%D9%84%D8%A7%D9%82_BBM', 'promoted_content': None, 'query': '%23%D8%A7%D8%BA%D9%84%D8%A7%D9%82_BBM', 'tweet_volume': 17055}, {'name': '#19aprile', 'url': 'http://twitter.com/search?q=%2319aprile', 'promoted_content': None, 'query': '%2319aprile', 'tweet_volume': None}, {'name': '#CHIvLIO', 'url': 'http://twitter.com/search?q=%23CHIvLIO', 'promoted_content': None, 'query': '%23CHIvLIO', 'tweet_volume': None}, {'name': '#Karfreitag', 'url': 'http://twitter.com/search?q=%23Karfreitag', 'promoted_content': None, 'query': '%23Karfreitag', 'tweet_volume': None}, {'name': '#JunquerasACN', 'url': 'http://twitter.com/search?q=%23JunquerasACN', 'promoted_content': None, 'query': '%23JunquerasACN', 'tweet_volume': None}], 'as_of': '2019-04-19T08:43:43Z', 'created_at': '2019-04-19T08:39:15Z', 'locations': [{'name': 'Worldwide', 'woeid': 1}]}]
    [{'trends': [{'name': '#WeLoveTheEarth', 'url': 'http://twitter.com/search?q=%23WeLoveTheEarth', 'promoted_content': None, 'query': '%23WeLoveTheEarth', 'tweet_volume': 159698}, {'name': '#DragRace', 'url': 'http://twitter.com/search?q=%23DragRace', 'promoted_content': None, 'query': '%23DragRace', 'tweet_volume': 37166}, {'name': 'Lil Dicky', 'url': 'http://twitter.com/search?q=%22Lil+Dicky%22', 'promoted_content': None, 'query': '%22Lil+Dicky%22', 'tweet_volume': 42461}, {'name': 'Derrick White', 'url': 'http://twitter.com/search?q=%22Derrick+White%22', 'promoted_content': None, 'query': '%22Derrick+White%22', 'tweet_volume': 27104}, {'name': '#CUZILOVEYOU', 'url': 'http://twitter.com/search?q=%23CUZILOVEYOU', 'promoted_content': None, 'query': '%23CUZILOVEYOU', 'tweet_volume': None}, {'name': 'Kevin Durant', 'url': 'http://twitter.com/search?q=%22Kevin+Durant%22', 'promoted_content': None, 'query': '%22Kevin+Durant%22', 'tweet_volume': 21870}, {'name': '#StarTrekDiscovery', 'url': 'http://twitter.com/search?q=%23StarTrekDiscovery', 'promoted_content': None, 'query': '%23StarTrekDiscovery', 'tweet_volume': None}, {'name': '#GSWvsLAC', 'url': 'http://twitter.com/search?q=%23GSWvsLAC', 'promoted_content': None, 'query': '%23GSWvsLAC', 'tweet_volume': None}, {'name': 'Oshie', 'url': 'http://twitter.com/search?q=Oshie', 'promoted_content': None, 'query': 'Oshie', 'tweet_volume': None}, {'name': 'Seth Abramson', 'url': 'http://twitter.com/search?q=%22Seth+Abramson%22', 'promoted_content': None, 'query': '%22Seth+Abramson%22', 'tweet_volume': None}, {'name': 'Lyra McKee', 'url': 'http://twitter.com/search?q=%22Lyra+McKee%22', 'promoted_content': None, 'query': '%22Lyra+McKee%22', 'tweet_volume': 17606}, {'name': 'Silky', 'url': 'http://twitter.com/search?q=Silky', 'promoted_content': None, 'query': 'Silky', 'tweet_volume': 12881}, {'name': 'Kazuo Koike', 'url': 'http://twitter.com/search?q=%22Kazuo+Koike%22', 'promoted_content': None, 'query': '%22Kazuo+Koike%22', 'tweet_volume': None}, {'name': 'Game 6', 'url': 'http://twitter.com/search?q=%22Game+6%22', 'promoted_content': None, 'query': '%22Game+6%22', 'tweet_volume': None}, {'name': 'Yvie', 'url': 'http://twitter.com/search?q=Yvie', 'promoted_content': None, 'query': 'Yvie', 'tweet_volume': 10680}, {'name': 'Gallant', 'url': 'http://twitter.com/search?q=Gallant', 'promoted_content': None, 'query': 'Gallant', 'tweet_volume': None}, {'name': 'Lone Wolf and Cub', 'url': 'http://twitter.com/search?q=%22Lone+Wolf+and+Cub%22', 'promoted_content': None, 'query': '%22Lone+Wolf+and+Cub%22', 'tweet_volume': None}, {'name': 'George Conway', 'url': 'http://twitter.com/search?q=%22George+Conway%22', 'promoted_content': None, 'query': '%22George+Conway%22', 'tweet_volume': 27458}, {'name': 'David Fletcher', 'url': 'http://twitter.com/search?q=%22David+Fletcher%22', 'promoted_content': None, 'query': '%22David+Fletcher%22', 'tweet_volume': None}, {'name': 'Derry', 'url': 'http://twitter.com/search?q=Derry', 'promoted_content': None, 'query': 'Derry', 'tweet_volume': 28234}, {'name': 'Mike Anderson', 'url': 'http://twitter.com/search?q=%22Mike+Anderson%22', 'promoted_content': None, 'query': '%22Mike+Anderson%22', 'tweet_volume': None}, {'name': 'Shy Glizzy', 'url': 'http://twitter.com/search?q=%22Shy+Glizzy%22', 'promoted_content': None, 'query': '%22Shy+Glizzy%22', 'tweet_volume': None}, {'name': 'Tomas Hertl', 'url': 'http://twitter.com/search?q=%22Tomas+Hertl%22', 'promoted_content': None, 'query': '%22Tomas+Hertl%22', 'tweet_volume': None}, {'name': 'Servais', 'url': 'http://twitter.com/search?q=Servais', 'promoted_content': None, 'query': 'Servais', 'tweet_volume': None}, {'name': 'WE LOVE THE EARTH', 'url': 'http://twitter.com/search?q=%22WE+LOVE+THE+EARTH%22', 'promoted_content': None, 'query': '%22WE+LOVE+THE+EARTH%22', 'tweet_volume': None}, {'name': '"Earth"', 'url': 'http://twitter.com/search?q=%22Earth%22', 'promoted_content': None, 'query': '%22Earth%22', 'tweet_volume': 338417}, {'name': '#DinahJane1', 'url': 'http://twitter.com/search?q=%23DinahJane1', 'promoted_content': None, 'query': '%23DinahJane1', 'tweet_volume': 23757}, {'name': '#WhatStopsYouFromGoingHome', 'url': 'http://twitter.com/search?q=%23WhatStopsYouFromGoingHome', 'promoted_content': None, 'query': '%23WhatStopsYouFromGoingHome', 'tweet_volume': None}, {'name': '#MakeAMovieSensual', 'url': 'http://twitter.com/search?q=%23MakeAMovieSensual', 'promoted_content': None, 'query': '%23MakeAMovieSensual', 'tweet_volume': None}, {'name': '#DontChangeOutNow', 'url': 'http://twitter.com/search?q=%23DontChangeOutNow', 'promoted_content': None, 'query': '%23DontChangeOutNow', 'tweet_volume': None}, {'name': '#BLACKPINKxCorden', 'url': 'http://twitter.com/search?q=%23BLACKPINKxCorden', 'promoted_content': None, 'query': '%23BLACKPINKxCorden', 'tweet_volume': 253605}, {'name': '#WorldofWarcraftMains', 'url': 'http://twitter.com/search?q=%23WorldofWarcraftMains', 'promoted_content': None, 'query': '%23WorldofWarcraftMains', 'tweet_volume': None}, {'name': '#MyDrunkUncleSays', 'url': 'http://twitter.com/search?q=%23MyDrunkUncleSays', 'promoted_content': None, 'query': '%23MyDrunkUncleSays', 'tweet_volume': None}, {'name': '#WGAMIX', 'url': 'http://twitter.com/search?q=%23WGAMIX', 'promoted_content': None, 'query': '%23WGAMIX', 'tweet_volume': None}, {'name': '#Earth', 'url': 'http://twitter.com/search?q=%23Earth', 'promoted_content': None, 'query': '%23Earth', 'tweet_volume': 13655}, {'name': '#TheLegendOfVoxMachina', 'url': 'http://twitter.com/search?q=%23TheLegendOfVoxMachina', 'promoted_content': None, 'query': '%23TheLegendOfVoxMachina', 'tweet_volume': None}, {'name': '#AFLNorthDons', 'url': 'http://twitter.com/search?q=%23AFLNorthDons', 'promoted_content': None, 'query': '%23AFLNorthDons', 'tweet_volume': None}, {'name': '#FridayFeeling', 'url': 'http://twitter.com/search?q=%23FridayFeeling', 'promoted_content': None, 'query': '%23FridayFeeling', 'tweet_volume': 19510}, {'name': '#MyInnerDemonSaid', 'url': 'http://twitter.com/search?q=%23MyInnerDemonSaid', 'promoted_content': None, 'query': '%23MyInnerDemonSaid', 'tweet_volume': None}, {'name': '#rupaulsdragrace', 'url': 'http://twitter.com/search?q=%23rupaulsdragrace', 'promoted_content': None, 'query': '%23rupaulsdragrace', 'tweet_volume': None}, {'name': '#ConCalmaRemix', 'url': 'http://twitter.com/search?q=%23ConCalmaRemix', 'promoted_content': None, 'query': '%23ConCalmaRemix', 'tweet_volume': 37846}, {'name': '#TimeToImpeach', 'url': 'http://twitter.com/search?q=%23TimeToImpeach', 'promoted_content': None, 'query': '%23TimeToImpeach', 'tweet_volume': 21732}, {'name': '#NRLBulldogsSouths', 'url': 'http://twitter.com/search?q=%23NRLBulldogsSouths', 'promoted_content': None, 'query': '%23NRLBulldogsSouths', 'tweet_volume': None}, {'name': '#CriticalRoleSpoilers', 'url': 'http://twitter.com/search?q=%23CriticalRoleSpoilers', 'promoted_content': None, 'query': '%23CriticalRoleSpoilers', 'tweet_volume': None}, {'name': '#GossipShouldBe', 'url': 'http://twitter.com/search?q=%23GossipShouldBe', 'promoted_content': None, 'query': '%23GossipShouldBe', 'tweet_volume': None}, {'name': '#LilDicky', 'url': 'http://twitter.com/search?q=%23LilDicky', 'promoted_content': None, 'query': '%23LilDicky', 'tweet_volume': None}, {'name': '#RPDR', 'url': 'http://twitter.com/search?q=%23RPDR', 'promoted_content': None, 'query': '%23RPDR', 'tweet_volume': None}, {'name': '#WeirdDateStories', 'url': 'http://twitter.com/search?q=%23WeirdDateStories', 'promoted_content': None, 'query': '%23WeirdDateStories', 'tweet_volume': None}, {'name': '#HustleAndSoul', 'url': 'http://twitter.com/search?q=%23HustleAndSoul', 'promoted_content': None, 'query': '%23HustleAndSoul', 'tweet_volume': None}, {'name': '#fridaymotivation', 'url': 'http://twitter.com/search?q=%23fridaymotivation', 'promoted_content': None, 'query': '%23fridaymotivation', 'tweet_volume': None}], 'as_of': '2019-04-19T08:43:43Z', 'created_at': '2019-04-19T08:39:15Z', 'locations': [{'name': 'United States', 'woeid': 23424977}]}]


## 2. Prettifying the output
<p>Our data was hard to read! Luckily, we can resort to the <i>json.dumps()</i> method to have it formatted as a pretty JSON string.</p>


```python
# Pretty-printing the results. First WW and then US trends.

print("WW trends:")
print(json.dumps(WW_trends,indent = 1))

print("\n", "US trends:")
print(json.dumps(US_trends,indent = 1))
```

    WW trends:
    [
     {
      "trends": [
       {
        "name": "#BeratKandili",
        "url": "http://twitter.com/search?q=%23BeratKandili",
        "promoted_content": null,
        "query": "%23BeratKandili",
        "tweet_volume": 46373
       },
       {
        "name": "#GoodFriday",
        "url": "http://twitter.com/search?q=%23GoodFriday",
        "promoted_content": null,
        "query": "%23GoodFriday",
        "tweet_volume": 81891
       },
       {
        "name": "#WeLoveTheEarth",
        "url": "http://twitter.com/search?q=%23WeLoveTheEarth",
        "promoted_content": null,
        "query": "%23WeLoveTheEarth",
        "tweet_volume": 159698
       },
       {
        "name": "#195TLdenTTVerilir",
        "url": "http://twitter.com/search?q=%23195TLdenTTVerilir",
        "promoted_content": null,
        "query": "%23195TLdenTTVerilir",
        "tweet_volume": null
       },
       {
        "name": "#AFLNorthDons",
        "url": "http://twitter.com/search?q=%23AFLNorthDons",
        "promoted_content": null,
        "query": "%23AFLNorthDons",
        "tweet_volume": null
       },
       {
        "name": "Shiv Sena",
        "url": "http://twitter.com/search?q=%22Shiv+Sena%22",
        "promoted_content": null,
        "query": "%22Shiv+Sena%22",
        "tweet_volume": null
       },
       {
        "name": "Lyra McKee",
        "url": "http://twitter.com/search?q=%22Lyra+McKee%22",
        "promoted_content": null,
        "query": "%22Lyra+McKee%22",
        "tweet_volume": 17606
       },
       {
        "name": "Priyanka Chaturvedi",
        "url": "http://twitter.com/search?q=%22Priyanka+Chaturvedi%22",
        "promoted_content": null,
        "query": "%22Priyanka+Chaturvedi%22",
        "tweet_volume": 22342
       },
       {
        "name": "Derry",
        "url": "http://twitter.com/search?q=Derry",
        "promoted_content": null,
        "query": "Derry",
        "tweet_volume": 28234
       },
       {
        "name": "\u6c60\u888b\u306e\u4e8b\u6545",
        "url": "http://twitter.com/search?q=%E6%B1%A0%E8%A2%8B%E3%81%AE%E4%BA%8B%E6%95%85",
        "promoted_content": null,
        "query": "%E6%B1%A0%E8%A2%8B%E3%81%AE%E4%BA%8B%E6%95%85",
        "tweet_volume": 34381
       },
       {
        "name": "\u30d7\u30ea\u30a6\u30b9",
        "url": "http://twitter.com/search?q=%E3%83%97%E3%83%AA%E3%82%A6%E3%82%B9",
        "promoted_content": null,
        "query": "%E3%83%97%E3%83%AA%E3%82%A6%E3%82%B9",
        "tweet_volume": 22944
       },
       {
        "name": "Hemant Karkare",
        "url": "http://twitter.com/search?q=%22Hemant+Karkare%22",
        "promoted_content": null,
        "query": "%22Hemant+Karkare%22",
        "tweet_volume": 24067
       },
       {
        "name": "\u9ad8\u9f62\u8005",
        "url": "http://twitter.com/search?q=%E9%AB%98%E9%BD%A2%E8%80%85",
        "promoted_content": null,
        "query": "%E9%AB%98%E9%BD%A2%E8%80%85",
        "tweet_volume": 28382
       },
       {
        "name": "\ube0c\uc774\uc54c",
        "url": "http://twitter.com/search?q=%EB%B8%8C%EC%9D%B4%EC%95%8C",
        "promoted_content": null,
        "query": "%EB%B8%8C%EC%9D%B4%EC%95%8C",
        "tweet_volume": 15490
       },
       {
        "name": "\u5200\u30b9\u30c6",
        "url": "http://twitter.com/search?q=%E5%88%80%E3%82%B9%E3%83%86",
        "promoted_content": null,
        "query": "%E5%88%80%E3%82%B9%E3%83%86",
        "tweet_volume": null
       },
       {
        "name": "\u514d\u8a31\u8fd4\u7d0d",
        "url": "http://twitter.com/search?q=%E5%85%8D%E8%A8%B1%E8%BF%94%E7%B4%8D",
        "promoted_content": null,
        "query": "%E5%85%8D%E8%A8%B1%E8%BF%94%E7%B4%8D",
        "tweet_volume": null
       },
       {
        "name": "Berat Kandilimiz",
        "url": "http://twitter.com/search?q=%22Berat+Kandilimiz%22",
        "promoted_content": null,
        "query": "%22Berat+Kandilimiz%22",
        "tweet_volume": 10901
       },
       {
        "name": "\u00f6rg\u00fctde\u011fil arkada\u015fgrubu",
        "url": "http://twitter.com/search?q=%22%C3%B6rg%C3%BCtde%C4%9Fil+arkada%C5%9Fgrubu%22",
        "promoted_content": null,
        "query": "%22%C3%B6rg%C3%BCtde%C4%9Fil+arkada%C5%9Fgrubu%22",
        "tweet_volume": null
       },
       {
        "name": "\u30b0\u30ec\u30a2",
        "url": "http://twitter.com/search?q=%E3%82%B0%E3%83%AC%E3%82%A2",
        "promoted_content": null,
        "query": "%E3%82%B0%E3%83%AC%E3%82%A2",
        "tweet_volume": 23485
       },
       {
        "name": "\u6771\u4eac\u30fb\u6c60\u888b\u885d\u7a81\u4e8b\u6545",
        "url": "http://twitter.com/search?q=%E6%9D%B1%E4%BA%AC%E3%83%BB%E6%B1%A0%E8%A2%8B%E8%A1%9D%E7%AA%81%E4%BA%8B%E6%95%85",
        "promoted_content": null,
        "query": "%E6%9D%B1%E4%BA%AC%E3%83%BB%E6%B1%A0%E8%A2%8B%E8%A1%9D%E7%AA%81%E4%BA%8B%E6%95%85",
        "tweet_volume": null
       },
       {
        "name": "\u91cd\u4f53\u306e\u5973\u6027\u3068\u5973\u5150",
        "url": "http://twitter.com/search?q=%E9%87%8D%E4%BD%93%E3%81%AE%E5%A5%B3%E6%80%A7%E3%81%A8%E5%A5%B3%E5%85%90",
        "promoted_content": null,
        "query": "%E9%87%8D%E4%BD%93%E3%81%AE%E5%A5%B3%E6%80%A7%E3%81%A8%E5%A5%B3%E5%85%90",
        "tweet_volume": null
       },
       {
        "name": "Lil Dicky",
        "url": "http://twitter.com/search?q=%22Lil+Dicky%22",
        "promoted_content": null,
        "query": "%22Lil+Dicky%22",
        "tweet_volume": 42461
       },
       {
        "name": "\u6b69\u884c\u8005",
        "url": "http://twitter.com/search?q=%E6%AD%A9%E8%A1%8C%E8%80%85",
        "promoted_content": null,
        "query": "%E6%AD%A9%E8%A1%8C%E8%80%85",
        "tweet_volume": 25405
       },
       {
        "name": "Derrick White",
        "url": "http://twitter.com/search?q=%22Derrick+White%22",
        "promoted_content": null,
        "query": "%22Derrick+White%22",
        "tweet_volume": 27104
       },
       {
        "name": "\u5341\u4e8c\u56fd\u8a18",
        "url": "http://twitter.com/search?q=%E5%8D%81%E4%BA%8C%E5%9B%BD%E8%A8%98",
        "promoted_content": null,
        "query": "%E5%8D%81%E4%BA%8C%E5%9B%BD%E8%A8%98",
        "tweet_volume": 46803
       },
       {
        "name": "#KpuJanganCurang",
        "url": "http://twitter.com/search?q=%23KpuJanganCurang",
        "promoted_content": null,
        "query": "%23KpuJanganCurang",
        "tweet_volume": 75384
       },
       {
        "name": "#Hay\u0131rl\u0131Cumalar",
        "url": "http://twitter.com/search?q=%23Hay%C4%B1rl%C4%B1Cumalar",
        "promoted_content": null,
        "query": "%23Hay%C4%B1rl%C4%B1Cumalar",
        "tweet_volume": 19848
       },
       {
        "name": "#Hay\u0131rl\u0131Kandiller",
        "url": "http://twitter.com/search?q=%23Hay%C4%B1rl%C4%B1Kandiller",
        "promoted_content": null,
        "query": "%23Hay%C4%B1rl%C4%B1Kandiller",
        "tweet_volume": null
       },
       {
        "name": "#HanumanJayanti",
        "url": "http://twitter.com/search?q=%23HanumanJayanti",
        "promoted_content": null,
        "query": "%23HanumanJayanti",
        "tweet_volume": 83138
       },
       {
        "name": "#IndonesianElectionHeroes",
        "url": "http://twitter.com/search?q=%23IndonesianElectionHeroes",
        "promoted_content": null,
        "query": "%23IndonesianElectionHeroes",
        "tweet_volume": 19664
       },
       {
        "name": "#\u064a\u0648\u0645_\u0627\u0644\u062c\u0645\u0639\u0647",
        "url": "http://twitter.com/search?q=%23%D9%8A%D9%88%D9%85_%D8%A7%D9%84%D8%AC%D9%85%D8%B9%D9%87",
        "promoted_content": null,
        "query": "%23%D9%8A%D9%88%D9%85_%D8%A7%D9%84%D8%AC%D9%85%D8%B9%D9%87",
        "tweet_volume": 80799
       },
       {
        "name": "#NRLBulldogsSouths",
        "url": "http://twitter.com/search?q=%23NRLBulldogsSouths",
        "promoted_content": null,
        "query": "%23NRLBulldogsSouths",
        "tweet_volume": null
       },
       {
        "name": "#NikahUmurBerapa",
        "url": "http://twitter.com/search?q=%23NikahUmurBerapa",
        "promoted_content": null,
        "query": "%23NikahUmurBerapa",
        "tweet_volume": null
       },
       {
        "name": "#DragRace",
        "url": "http://twitter.com/search?q=%23DragRace",
        "promoted_content": null,
        "query": "%23DragRace",
        "tweet_volume": 37166
       },
       {
        "name": "#ViernesSanto",
        "url": "http://twitter.com/search?q=%23ViernesSanto",
        "promoted_content": null,
        "query": "%23ViernesSanto",
        "tweet_volume": null
       },
       {
        "name": "#HardikPatel",
        "url": "http://twitter.com/search?q=%23HardikPatel",
        "promoted_content": null,
        "query": "%23HardikPatel",
        "tweet_volume": null
       },
       {
        "name": "#BLACKPINKxCorden",
        "url": "http://twitter.com/search?q=%23BLACKPINKxCorden",
        "promoted_content": null,
        "query": "%23BLACKPINKxCorden",
        "tweet_volume": 253605
       },
       {
        "name": "#Ontas",
        "url": "http://twitter.com/search?q=%23Ontas",
        "promoted_content": null,
        "query": "%23Ontas",
        "tweet_volume": 27924
       },
       {
        "name": "#ConCalmaRemix",
        "url": "http://twitter.com/search?q=%23ConCalmaRemix",
        "promoted_content": null,
        "query": "%23ConCalmaRemix",
        "tweet_volume": 37846
       },
       {
        "name": "#ProtestoEdiyorum",
        "url": "http://twitter.com/search?q=%23ProtestoEdiyorum",
        "promoted_content": null,
        "query": "%23ProtestoEdiyorum",
        "tweet_volume": null
       },
       {
        "name": "#DinahJane1",
        "url": "http://twitter.com/search?q=%23DinahJane1",
        "promoted_content": null,
        "query": "%23DinahJane1",
        "tweet_volume": 23757
       },
       {
        "name": "#ShivSena",
        "url": "http://twitter.com/search?q=%23ShivSena",
        "promoted_content": null,
        "query": "%23ShivSena",
        "tweet_volume": null
       },
       {
        "name": "#DuyguAsena",
        "url": "http://twitter.com/search?q=%23DuyguAsena",
        "promoted_content": null,
        "query": "%23DuyguAsena",
        "tweet_volume": null
       },
       {
        "name": "#TheJudasInMyLife",
        "url": "http://twitter.com/search?q=%23TheJudasInMyLife",
        "promoted_content": null,
        "query": "%23TheJudasInMyLife",
        "tweet_volume": null
       },
       {
        "name": "#Jersey",
        "url": "http://twitter.com/search?q=%23Jersey",
        "promoted_content": null,
        "query": "%23Jersey",
        "tweet_volume": 20509
       },
       {
        "name": "#\u0627\u063a\u0644\u0627\u0642_BBM",
        "url": "http://twitter.com/search?q=%23%D8%A7%D8%BA%D9%84%D8%A7%D9%82_BBM",
        "promoted_content": null,
        "query": "%23%D8%A7%D8%BA%D9%84%D8%A7%D9%82_BBM",
        "tweet_volume": 17055
       },
       {
        "name": "#19aprile",
        "url": "http://twitter.com/search?q=%2319aprile",
        "promoted_content": null,
        "query": "%2319aprile",
        "tweet_volume": null
       },
       {
        "name": "#CHIvLIO",
        "url": "http://twitter.com/search?q=%23CHIvLIO",
        "promoted_content": null,
        "query": "%23CHIvLIO",
        "tweet_volume": null
       },
       {
        "name": "#Karfreitag",
        "url": "http://twitter.com/search?q=%23Karfreitag",
        "promoted_content": null,
        "query": "%23Karfreitag",
        "tweet_volume": null
       },
       {
        "name": "#JunquerasACN",
        "url": "http://twitter.com/search?q=%23JunquerasACN",
        "promoted_content": null,
        "query": "%23JunquerasACN",
        "tweet_volume": null
       }
      ],
      "as_of": "2019-04-19T08:43:43Z",
      "created_at": "2019-04-19T08:39:15Z",
      "locations": [
       {
        "name": "Worldwide",
        "woeid": 1
       }
      ]
     }
    ]
    
     US trends:
    [
     {
      "trends": [
       {
        "name": "#WeLoveTheEarth",
        "url": "http://twitter.com/search?q=%23WeLoveTheEarth",
        "promoted_content": null,
        "query": "%23WeLoveTheEarth",
        "tweet_volume": 159698
       },
       {
        "name": "#DragRace",
        "url": "http://twitter.com/search?q=%23DragRace",
        "promoted_content": null,
        "query": "%23DragRace",
        "tweet_volume": 37166
       },
       {
        "name": "Lil Dicky",
        "url": "http://twitter.com/search?q=%22Lil+Dicky%22",
        "promoted_content": null,
        "query": "%22Lil+Dicky%22",
        "tweet_volume": 42461
       },
       {
        "name": "Derrick White",
        "url": "http://twitter.com/search?q=%22Derrick+White%22",
        "promoted_content": null,
        "query": "%22Derrick+White%22",
        "tweet_volume": 27104
       },
       {
        "name": "#CUZILOVEYOU",
        "url": "http://twitter.com/search?q=%23CUZILOVEYOU",
        "promoted_content": null,
        "query": "%23CUZILOVEYOU",
        "tweet_volume": null
       },
       {
        "name": "Kevin Durant",
        "url": "http://twitter.com/search?q=%22Kevin+Durant%22",
        "promoted_content": null,
        "query": "%22Kevin+Durant%22",
        "tweet_volume": 21870
       },
       {
        "name": "#StarTrekDiscovery",
        "url": "http://twitter.com/search?q=%23StarTrekDiscovery",
        "promoted_content": null,
        "query": "%23StarTrekDiscovery",
        "tweet_volume": null
       },
       {
        "name": "#GSWvsLAC",
        "url": "http://twitter.com/search?q=%23GSWvsLAC",
        "promoted_content": null,
        "query": "%23GSWvsLAC",
        "tweet_volume": null
       },
       {
        "name": "Oshie",
        "url": "http://twitter.com/search?q=Oshie",
        "promoted_content": null,
        "query": "Oshie",
        "tweet_volume": null
       },
       {
        "name": "Seth Abramson",
        "url": "http://twitter.com/search?q=%22Seth+Abramson%22",
        "promoted_content": null,
        "query": "%22Seth+Abramson%22",
        "tweet_volume": null
       },
       {
        "name": "Lyra McKee",
        "url": "http://twitter.com/search?q=%22Lyra+McKee%22",
        "promoted_content": null,
        "query": "%22Lyra+McKee%22",
        "tweet_volume": 17606
       },
       {
        "name": "Silky",
        "url": "http://twitter.com/search?q=Silky",
        "promoted_content": null,
        "query": "Silky",
        "tweet_volume": 12881
       },
       {
        "name": "Kazuo Koike",
        "url": "http://twitter.com/search?q=%22Kazuo+Koike%22",
        "promoted_content": null,
        "query": "%22Kazuo+Koike%22",
        "tweet_volume": null
       },
       {
        "name": "Game 6",
        "url": "http://twitter.com/search?q=%22Game+6%22",
        "promoted_content": null,
        "query": "%22Game+6%22",
        "tweet_volume": null
       },
       {
        "name": "Yvie",
        "url": "http://twitter.com/search?q=Yvie",
        "promoted_content": null,
        "query": "Yvie",
        "tweet_volume": 10680
       },
       {
        "name": "Gallant",
        "url": "http://twitter.com/search?q=Gallant",
        "promoted_content": null,
        "query": "Gallant",
        "tweet_volume": null
       },
       {
        "name": "Lone Wolf and Cub",
        "url": "http://twitter.com/search?q=%22Lone+Wolf+and+Cub%22",
        "promoted_content": null,
        "query": "%22Lone+Wolf+and+Cub%22",
        "tweet_volume": null
       },
       {
        "name": "George Conway",
        "url": "http://twitter.com/search?q=%22George+Conway%22",
        "promoted_content": null,
        "query": "%22George+Conway%22",
        "tweet_volume": 27458
       },
       {
        "name": "David Fletcher",
        "url": "http://twitter.com/search?q=%22David+Fletcher%22",
        "promoted_content": null,
        "query": "%22David+Fletcher%22",
        "tweet_volume": null
       },
       {
        "name": "Derry",
        "url": "http://twitter.com/search?q=Derry",
        "promoted_content": null,
        "query": "Derry",
        "tweet_volume": 28234
       },
       {
        "name": "Mike Anderson",
        "url": "http://twitter.com/search?q=%22Mike+Anderson%22",
        "promoted_content": null,
        "query": "%22Mike+Anderson%22",
        "tweet_volume": null
       },
       {
        "name": "Shy Glizzy",
        "url": "http://twitter.com/search?q=%22Shy+Glizzy%22",
        "promoted_content": null,
        "query": "%22Shy+Glizzy%22",
        "tweet_volume": null
       },
       {
        "name": "Tomas Hertl",
        "url": "http://twitter.com/search?q=%22Tomas+Hertl%22",
        "promoted_content": null,
        "query": "%22Tomas+Hertl%22",
        "tweet_volume": null
       },
       {
        "name": "Servais",
        "url": "http://twitter.com/search?q=Servais",
        "promoted_content": null,
        "query": "Servais",
        "tweet_volume": null
       },
       {
        "name": "WE LOVE THE EARTH",
        "url": "http://twitter.com/search?q=%22WE+LOVE+THE+EARTH%22",
        "promoted_content": null,
        "query": "%22WE+LOVE+THE+EARTH%22",
        "tweet_volume": null
       },
       {
        "name": "\"Earth\"",
        "url": "http://twitter.com/search?q=%22Earth%22",
        "promoted_content": null,
        "query": "%22Earth%22",
        "tweet_volume": 338417
       },
       {
        "name": "#DinahJane1",
        "url": "http://twitter.com/search?q=%23DinahJane1",
        "promoted_content": null,
        "query": "%23DinahJane1",
        "tweet_volume": 23757
       },
       {
        "name": "#WhatStopsYouFromGoingHome",
        "url": "http://twitter.com/search?q=%23WhatStopsYouFromGoingHome",
        "promoted_content": null,
        "query": "%23WhatStopsYouFromGoingHome",
        "tweet_volume": null
       },
       {
        "name": "#MakeAMovieSensual",
        "url": "http://twitter.com/search?q=%23MakeAMovieSensual",
        "promoted_content": null,
        "query": "%23MakeAMovieSensual",
        "tweet_volume": null
       },
       {
        "name": "#DontChangeOutNow",
        "url": "http://twitter.com/search?q=%23DontChangeOutNow",
        "promoted_content": null,
        "query": "%23DontChangeOutNow",
        "tweet_volume": null
       },
       {
        "name": "#BLACKPINKxCorden",
        "url": "http://twitter.com/search?q=%23BLACKPINKxCorden",
        "promoted_content": null,
        "query": "%23BLACKPINKxCorden",
        "tweet_volume": 253605
       },
       {
        "name": "#WorldofWarcraftMains",
        "url": "http://twitter.com/search?q=%23WorldofWarcraftMains",
        "promoted_content": null,
        "query": "%23WorldofWarcraftMains",
        "tweet_volume": null
       },
       {
        "name": "#MyDrunkUncleSays",
        "url": "http://twitter.com/search?q=%23MyDrunkUncleSays",
        "promoted_content": null,
        "query": "%23MyDrunkUncleSays",
        "tweet_volume": null
       },
       {
        "name": "#WGAMIX",
        "url": "http://twitter.com/search?q=%23WGAMIX",
        "promoted_content": null,
        "query": "%23WGAMIX",
        "tweet_volume": null
       },
       {
        "name": "#Earth",
        "url": "http://twitter.com/search?q=%23Earth",
        "promoted_content": null,
        "query": "%23Earth",
        "tweet_volume": 13655
       },
       {
        "name": "#TheLegendOfVoxMachina",
        "url": "http://twitter.com/search?q=%23TheLegendOfVoxMachina",
        "promoted_content": null,
        "query": "%23TheLegendOfVoxMachina",
        "tweet_volume": null
       },
       {
        "name": "#AFLNorthDons",
        "url": "http://twitter.com/search?q=%23AFLNorthDons",
        "promoted_content": null,
        "query": "%23AFLNorthDons",
        "tweet_volume": null
       },
       {
        "name": "#FridayFeeling",
        "url": "http://twitter.com/search?q=%23FridayFeeling",
        "promoted_content": null,
        "query": "%23FridayFeeling",
        "tweet_volume": 19510
       },
       {
        "name": "#MyInnerDemonSaid",
        "url": "http://twitter.com/search?q=%23MyInnerDemonSaid",
        "promoted_content": null,
        "query": "%23MyInnerDemonSaid",
        "tweet_volume": null
       },
       {
        "name": "#rupaulsdragrace",
        "url": "http://twitter.com/search?q=%23rupaulsdragrace",
        "promoted_content": null,
        "query": "%23rupaulsdragrace",
        "tweet_volume": null
       },
       {
        "name": "#ConCalmaRemix",
        "url": "http://twitter.com/search?q=%23ConCalmaRemix",
        "promoted_content": null,
        "query": "%23ConCalmaRemix",
        "tweet_volume": 37846
       },
       {
        "name": "#TimeToImpeach",
        "url": "http://twitter.com/search?q=%23TimeToImpeach",
        "promoted_content": null,
        "query": "%23TimeToImpeach",
        "tweet_volume": 21732
       },
       {
        "name": "#NRLBulldogsSouths",
        "url": "http://twitter.com/search?q=%23NRLBulldogsSouths",
        "promoted_content": null,
        "query": "%23NRLBulldogsSouths",
        "tweet_volume": null
       },
       {
        "name": "#CriticalRoleSpoilers",
        "url": "http://twitter.com/search?q=%23CriticalRoleSpoilers",
        "promoted_content": null,
        "query": "%23CriticalRoleSpoilers",
        "tweet_volume": null
       },
       {
        "name": "#GossipShouldBe",
        "url": "http://twitter.com/search?q=%23GossipShouldBe",
        "promoted_content": null,
        "query": "%23GossipShouldBe",
        "tweet_volume": null
       },
       {
        "name": "#LilDicky",
        "url": "http://twitter.com/search?q=%23LilDicky",
        "promoted_content": null,
        "query": "%23LilDicky",
        "tweet_volume": null
       },
       {
        "name": "#RPDR",
        "url": "http://twitter.com/search?q=%23RPDR",
        "promoted_content": null,
        "query": "%23RPDR",
        "tweet_volume": null
       },
       {
        "name": "#WeirdDateStories",
        "url": "http://twitter.com/search?q=%23WeirdDateStories",
        "promoted_content": null,
        "query": "%23WeirdDateStories",
        "tweet_volume": null
       },
       {
        "name": "#HustleAndSoul",
        "url": "http://twitter.com/search?q=%23HustleAndSoul",
        "promoted_content": null,
        "query": "%23HustleAndSoul",
        "tweet_volume": null
       },
       {
        "name": "#fridaymotivation",
        "url": "http://twitter.com/search?q=%23fridaymotivation",
        "promoted_content": null,
        "query": "%23fridaymotivation",
        "tweet_volume": null
       }
      ],
      "as_of": "2019-04-19T08:43:43Z",
      "created_at": "2019-04-19T08:39:15Z",
      "locations": [
       {
        "name": "United States",
        "woeid": 23424977
       }
      ]
     }
    ]

