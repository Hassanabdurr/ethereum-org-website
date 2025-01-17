---
title: రహస్య నేత ఎన్నిక
description: రహస్య నాయకుని ఎన్నిక దాడుల నుండి వ్యాలిడేటర్‌లను ఎలా రక్షించడంలో సహాయపడుతుంది అనే వివరణ
lang: te
summaryPoints:
  - బ్లాక్ ప్రపోజర్ల యొక్క IP చిరునామా ముందుగానే తెలుసుకోవచ్చు, తద్వారా వారు దాడులకు గురవుతారు
  - సీక్రెట్ లీడర్ ఎలక్షన్ చెల్లుబాటుదారుల గుర్తింపును దాచిపెడుతుంది, తద్వారా వారు ముందుగానే తెలుసుకోలేరు
  - ఈ ఆలోచన యొక్క పొడిగింపు ప్రతి స్లాట్‌లో వాలిడేటర్ ఎంపికను యాదృచ్ఛికంగా చేయడం.
---

# రహస్య నేత ఎన్నిక {#single-secret-leader-election}

నేటి [ప్రూఫ్-ఆఫ్-స్టాక్](/developers/docs/consensus-mechanisms/pos) ఆధారిత ఏకాభిప్రాయ విధానంలో, రాబోయే బ్లాక్ ప్రపోజర్‌ల జాబితా పబ్లిక్‌గా ఉంటుంది మరియు వారి IP చిరునామాలను మ్యాప్ చేయడం సాధ్యపడుతుంది. దీనర్థం, దాడి చేసేవారు బ్లాక్‌ను ప్రతిపాదించడానికి ఏ వాలిడేటర్‌లను గుర్తించగలరు మరియు వారిని సమయానికి తమ బ్లాక్‌ను ప్రతిపాదించలేక పోవడంతో డినియల్-ఆఫ్-సర్వీస్ (DOS) దాడితో వారిని లక్ష్యంగా చేసుకోవచ్చు.

ఇది దాడి చేసే వ్యక్తికి లాభం పొందే అవకాశాలను సృష్టించగలదు. ఉదాహరణకు స్లాట్ `n+1` కోసం ఎంచుకున్న బ్లాక్ ప్రపోజర్ `n` స్లాట్‌లో ప్రపోజర్‌ను డాస్ చేయగలదు, తద్వారా వారు బ్లాక్‌ను ప్రతిపాదించే అవకాశాన్ని కోల్పోతారు. ఇది దాడి చేసే బ్లాక్ ప్రపోజర్‌ను రెండు స్లాట్‌ల యొక్క MEVని సంగ్రహించడానికి లేదా రెండు బ్లాక్‌లలో విభజించబడిన అన్ని లావాదేవీలను పట్టుకోవడానికి అనుమతిస్తుంది మరియు బదులుగా వాటన్నింటినీ ఒకదానిలో చేర్చి, అన్ని అనుబంధ రుసుములను పొందుతుంది. ఇది DOS దాడుల నుండి తమను తాము రక్షించుకోవడానికి మరింత అధునాతన పద్ధతులను ఉపయోగించగల అధునాతన సంస్థాగత వాలిడేటర్‌ల కంటే హోమ్ వాలిడేటర్‌లను ఎక్కువగా ప్రభావితం చేసే అవకాశం ఉంది మరియు అందువల్ల ఇది కేంద్రీకృత శక్తి కావచ్చు.

ఈ సమస్యకు అనేక పరిష్కారాలు ఉన్నాయి. ఒకటి [డిస్ట్రిబ్యూటెడ్ వాలిడేటర్ టెక్నాలజీ](https://github.com/ethereum/distributed-validator-specs) ఇది రిడెండెన్సీతో బహుళ మెషీన్‌లలో వాలిడేటర్‌ను అమలు చేయడానికి సంబంధించిన వివిధ పనులను విస్తరించడం దీని లక్ష్యం, తద్వారా నిర్దిష్ట స్లాట్‌లో బ్లాక్‌ను ప్రతిపాదించకుండా నిరోధించడం దాడి చేసేవారికి చాలా కష్టం. అయితే, అత్యంత బలమైన పరిష్కారం **సింగిల్ సీక్రెట్ లీడర్ ఎలక్షన్ (SSLE)**.

## ఒకే రహస్య నేత ఎన్నిక {#secret-leader-election}

SSLEలో, ఎంచుకున్న వ్యాలిడేటర్‌కు మాత్రమే తాము ఎంపిక చేసినట్లు తెలుసుకునేలా తెలివైన క్రిప్టోగ్రఫీ ఉపయోగించబడుతుంది. ప్రతి వ్యాలిడేటర్ వారందరూ పంచుకునే రహస్యానికి నిబద్ధతను సమర్పించడం ద్వారా ఇది పని చేస్తుంది. కమిట్‌మెంట్‌లు షఫుల్ చేయబడ్డాయి మరియు రీకాన్ఫిగర్ చేయబడతాయి, తద్వారా ఎవరూ వాలిడేటర్‌లకు కమిట్‌మెంట్‌లను మ్యాప్ చేయలేరు కానీ ప్రతి వాలిడేటర్‌కు ఏ నిబద్ధత చెందుతుందో తెలుసు. అప్పుడు, ఒక నిబద్ధత యాదృచ్ఛికంగా ఎంపిక చేయబడుతుంది. ఒక వేలిడేటర్ వారి నిబద్ధత ఎంపిక చేయబడిందని గుర్తిస్తే, బ్లాక్‌ను ప్రతిపాదించడం తమ వంతు అని వారికి తెలుసు.

ఈ ఆలోచన యొక్క ప్రధాన అమలును [Whisk](https://ethresear.ch/t/whisk-a-practical-shuffle-based-ssle-protocol-for-ethereum/11763) అంటారు. ఏది క్రింది విధంగా పనిచేస్తుంది:

1. ధృవీకరణదారులు భాగస్వామ్య రహస్యానికి కట్టుబడి ఉంటారు. కమిట్‌మెంట్ స్కీమ్ రూపొందించబడింది, ఇది వాలిడేటర్ గుర్తింపుకు కట్టుబడి ఉంటుంది కానీ యాదృచ్ఛికంగా కూడా ఉంటుంది, తద్వారా ఏ మూడవ పక్షం బైండింగ్‌ను రివర్స్ ఇంజనీర్ చేయలేరు మరియు నిర్దిష్ట వాలిడేటర్‌కు నిర్దిష్ట నిబద్ధతను లింక్ చేయలేరు.
2. ఒక ఎపోచ్ ప్రారంభంలో, RANDAO ఉపయోగించి 16,384 వాలిడేటర్‌ల నుండి మాదిరి కమిట్‌మెంట్‌లకు యాదృచ్ఛిక వాలిడేటర్‌ల సెట్ ఎంచుకోబడుతుంది.
3. తదుపరి 8182 స్లాట్‌ల కోసం (1 రోజు), బ్లాక్ ప్రపోజర్‌లు వారి స్వంత ప్రైవేట్ ఎంట్రోపీని ఉపయోగించి కమిట్‌మెంట్‌ల ఉపసమితిని షఫుల్ చేస్తారు మరియు రాండమైజ్ చేస్తారు.
4. షఫ్లింగ్ పూర్తయిన తర్వాత, RANDAO నిబద్ధతల యొక్క ఆర్డర్ జాబితాను రూపొందించడానికి ఉపయోగించబడుతుంది. ఈ జాబితా Ethereum స్లాట్‌లలో మ్యాప్ చేయబడింది.
5. వాలిడేటర్లు వారి నిబద్ధత నిర్దిష్ట స్లాట్‌కు జోడించబడిందని చూస్తారు మరియు ఆ స్లాట్ వచ్చినప్పుడు వారు ఒక బ్లాక్‌ను ప్రతిపాదిస్తారు.
6. ఈ దశలను పునరావృతం చేయండి, తద్వారా స్లాట్‌లకు కమిట్‌మెంట్‌ల కేటాయింపు ఎల్లప్పుడూ ప్రస్తుత స్లాట్ కంటే చాలా ముందు ఉంటుంది.

ఇది DOS దాడుల సామర్థ్యాన్ని నిరోధిస్తూ, తదుపరి బ్లాక్‌ను ఏ నిర్దిష్ట వ్యాలిడేటర్ ప్రతిపాదిస్తారో ముందుగానే దాడి చేసేవారికి తెలియకుండా చేస్తుంది.

## రహస్య నాన్-సింగిల్ లీడర్ ఎలక్షన్ (SnSLE) {#secret-non-single-leader-election}

**రహస్య నాన్-సింగిల్ లీడర్ ఎలక్షన్ ( SnSLE)** అని పిలువబడే ప్రూఫ్-ఆఫ్-వర్క్ కింద బ్లాక్ ప్రపోజల్ ఎలా నిర్ణయించబడిందో అదే విధంగా, ప్రతి స్లాట్‌లో ప్రతి ఒక్కరు ఒక బ్లాక్‌ను ప్రతిపాదించే అవకాశం ఉన్న దృష్టాంతాన్ని రూపొందించే లక్ష్యంతో ఒక ప్రత్యేక ప్రతిపాదన కూడా ఉంది. దీన్ని చేయడానికి ఒక సాధారణ మార్గం ఏమిటంటే, నేటి ప్రోటోకాల్‌లో యాదృచ్ఛికంగా వాలిడేటర్‌లను ఎంచుకోవడానికి ఉపయోగించే RANDAO ఫంక్షన్‌ను ఉపయోగించడం. RANDAO ఆలోచన ఏమిటంటే, అనేక స్వతంత్ర వాలిడేటర్‌లు సమర్పించిన హ్యాష్‌లను కలపడం ద్వారా తగినంత యాదృచ్ఛిక సంఖ్య ఉత్పత్తి అవుతుంది. SnSLEలో, ఈ హాష్‌లు తదుపరి బ్లాక్ ప్రపోజర్‌ను ఎంచుకోవడానికి ఉపయోగించబడతాయి, ఉదాహరణకు అత్యల్ప-విలువ హాష్‌ను ఎంచుకోవడం ద్వారా. ప్రతి స్లాట్‌లో వ్యక్తిగత వ్యాలిడేటర్‌లు ఎంపిక చేయబడే సంభావ్యతను ట్యూన్ చేయడానికి చెల్లుబాటు అయ్యే హ్యాష్‌ల పరిధిని పరిమితం చేయవచ్చు. హాష్ తప్పనిసరిగా `2^256 * 5 / N` కంటే తక్కువగా ఉండాలి అని నొక్కి చెప్పడం ద్వారా `N` = సక్రియ వ్యాలిడేటర్‌ల సంఖ్య, ప్రతి స్లాట్‌లో ఏదైనా వ్యక్తిగత వ్యాలిడేటర్ ఎంపిక చేయబడే అవకాశం ఉంటుంది `5/N`. ఈ ఉదాహరణలో, ప్రతి స్లాట్‌లో కనీసం ఒక ప్రపోజర్ చెల్లుబాటు అయ్యే హాష్‌ను రూపొందించడానికి 99.3% అవకాశం ఉంటుంది.

## ప్రస్తుత పురోగతి {#current-progress}

సేల్ మరియు సేల్ రెండూ పరిశోధన దశలో ఉన్నాయి. రెండు ఆలోచనలకు ఇంకా ఖరారు చేసిన స్పెసిఫికేషన్ లేదు. SSLE మరియు SnSLE పోటీ ప్రతిపాదనలు రెండూ అమలు చేయబడలేదు. షిప్పింగ్ చేయడానికి ముందు వారికి పబ్లిక్ టెస్ట్‌నెట్‌లలో మరింత పరిశోధన మరియు అభివృద్ధి, ప్రోటోటైపింగ్ మరియు అమలు చేయడం అవసరం.

## Further reading {#further-reading}

- [SnSLE](https://ethresear.ch/t/secret-non-single-leader-election/11789)
