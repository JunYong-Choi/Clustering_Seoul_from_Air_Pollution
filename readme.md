## ***대기오염에 따른 서울시 지역 군집화***
### 서울시, 대기오염의 주범은?

***

- 대기오염 Data를 활용한 서울시 Clustering 프로젝트

- 활용방안
    - 각 군집의 위치 및 도로를 활용한 도로청소경로 생성 가능
    - 군집별로 다른 환경 정책의 시행으로 효율성 극대화

***

### ***Data***

![b68f6e4e01bffc3b5dbee2752d9237ea.jpg](attachment:b68f6e4e01bffc3b5dbee2752d9237ea.jpg)

출처 : https://opengov.seoul.go.kr/mediahub/16867197

- 서울시 대기오염 측정소 50곳의 3개년 일별 대기오염 Data
    - 이산화질소, 오존, 일산화탄소, 아황산가스, 미세먼지, 초미세먼지 6개의 Column
    - 2020년도, 2021년도, 2022년도 3개년 일별 평균 Data

***

### ***데이터 전처리***

- 시계열 Data의 특성을 없애는 방향으로 전처리 진행
    - 월별, 계절별, 년도별, 요일별, 상하반기별 평균 오염도 Columns 생성

***

### ***사용한 분석 기법***

- Linkage Method
    - 본 프로젝트에서는 Ward link를 사용하였습니다.

![ward.JPG](attachment:ward.JPG)

- K-Means Clustering
    - 이 분석기법을 활용하여 총 몇개의 군집으로 나눌지 결정하였습니다.
    - Using Elbow Method (6개로 선정)

![kmeans.JPG](attachment:kmeans.JPG)

- PCA
    - biplot을 그려 각 cluster들이 columns에 의해 잘 분류 되었는지 대략적으로 확인 할 수 있었습니다.

![biplot.JPG](attachment:biplot.JPG)

- Exploratory Factor Analysis
    - Data에서 각 cluster 별로 모든 column을 비교하기 보다는 비슷한 Factor끼리 묶어 비교할 수 있었습니다.
    - 결과 6개의 대기오염 종류를 3개의 Factor로 묶을 수 있었습니다.

![factor.JPG](attachment:factor.JPG)

- Factor_1
    - 미세먼지농도, 초미세먼지농도에 대한 factor loading 값이 큼
    - "미세먼지오염"이라고 명명
- Factor_2
    - 이산화질소농도, 일산화탄소농도, 아황산가스농도에 대한 factor loading 값이 큼
    - 자동차의 배기가스에서는 질산화물질(NOx), 일산화탄소(CO), 탄화수소(HC), 총먼지(TSP : Total Solid Particlate), 아황산가스(SO2) 등 대기를 오염시키는 주요 물질 배출
    - 따라서 "자동차환경오염"이라고 명명 
- Factor_3
    - 오존농도에 대한 factor loading 값이 큼
    - "오존오염"이라고 명명     

***

### ***Pipeline***

- Read Data
    - Data를 읽고 특성 및 shape을 확인합니다.  
  
  
- Data Cleansing & Feature Engineering
    - Data 정제 및 필요한 Feature을 만드는 작업을 수행합니다.  
    
    
- Analysis
    - 정제된 Data를 가지고 군집분석, PCA, 요인분석을 진행합니다.  
    
    
- Final Analysis and Troubleshooting
    - 최종 clustering 및 각 cluster의 특성에 대해 파악합니다.
    - 통합대기환경지수를 활용해 각 cluster별로 점수를 매겨 각 cluster의 특성을 파악하는 지표로 사용하였습니다.
    
    
- Visualisation with folium
    - folium 모듈을 사용해 실제 측정소의 위치 및 주변 도로에 대한 map을 시각화합니다.

***

### ***최종 Clustering***

![%ED%81%B4%EB%9F%AC%EC%8A%A4%ED%84%B0%EC%A7%80%EB%8F%84.jpg](attachment:%ED%81%B4%EB%9F%AC%EC%8A%A4%ED%84%B0%EC%A7%80%EB%8F%84.jpg)

서울시 대기오염의 특성에 맞춰 총 6개의 cluster로 분류할 수 있었습니다.

***

### ***folium mapping***


```python
# map
# 출발지점은 국민대학교입니다.
```




<div style="width:100%;"><div style="position:relative;width:100%;height:0;padding-bottom:60%;"><span style="color:#565656">Make this Notebook Trusted to load map: File -> Trust Notebook</span><iframe srcdoc="&lt;!DOCTYPE html&gt;
&lt;html&gt;
&lt;head&gt;

    &lt;meta http-equiv=&quot;content-type&quot; content=&quot;text/html; charset=UTF-8&quot; /&gt;

        &lt;script&gt;
            L_NO_TOUCH = false;
            L_DISABLE_3D = false;
        &lt;/script&gt;

    &lt;style&gt;html, body {width: 100%;height: 100%;margin: 0;padding: 0;}&lt;/style&gt;
    &lt;style&gt;#map {position:absolute;top:0;bottom:0;right:0;left:0;}&lt;/style&gt;
    &lt;script src=&quot;https://cdn.jsdelivr.net/npm/leaflet@1.9.3/dist/leaflet.js&quot;&gt;&lt;/script&gt;
    &lt;script src=&quot;https://code.jquery.com/jquery-1.12.4.min.js&quot;&gt;&lt;/script&gt;
    &lt;script src=&quot;https://cdn.jsdelivr.net/npm/bootstrap@5.2.2/dist/js/bootstrap.bundle.min.js&quot;&gt;&lt;/script&gt;
    &lt;script src=&quot;https://cdnjs.cloudflare.com/ajax/libs/Leaflet.awesome-markers/2.0.2/leaflet.awesome-markers.js&quot;&gt;&lt;/script&gt;
    &lt;link rel=&quot;stylesheet&quot; href=&quot;https://cdn.jsdelivr.net/npm/leaflet@1.9.3/dist/leaflet.css&quot;/&gt;
    &lt;link rel=&quot;stylesheet&quot; href=&quot;https://cdn.jsdelivr.net/npm/bootstrap@5.2.2/dist/css/bootstrap.min.css&quot;/&gt;
    &lt;link rel=&quot;stylesheet&quot; href=&quot;https://netdna.bootstrapcdn.com/bootstrap/3.0.0/css/bootstrap.min.css&quot;/&gt;
    &lt;link rel=&quot;stylesheet&quot; href=&quot;https://cdn.jsdelivr.net/npm/@fortawesome/fontawesome-free@6.2.0/css/all.min.css&quot;/&gt;
    &lt;link rel=&quot;stylesheet&quot; href=&quot;https://cdnjs.cloudflare.com/ajax/libs/Leaflet.awesome-markers/2.0.2/leaflet.awesome-markers.css&quot;/&gt;
    &lt;link rel=&quot;stylesheet&quot; href=&quot;https://cdn.jsdelivr.net/gh/python-visualization/folium/folium/templates/leaflet.awesome.rotate.min.css&quot;/&gt;

            &lt;meta name=&quot;viewport&quot; content=&quot;width=device-width,
                initial-scale=1.0, maximum-scale=1.0, user-scalable=no&quot; /&gt;
            &lt;style&gt;
                #map_7170e2328a0347478c80993f4a7ac4ce {
                    position: relative;
                    width: 100.0%;
                    height: 100.0%;
                    left: 0.0%;
                    top: 0.0%;
                }
                .leaflet-container { font-size: 1rem; }
            &lt;/style&gt;

&lt;/head&gt;
&lt;body&gt;


            &lt;div class=&quot;folium-map&quot; id=&quot;map_7170e2328a0347478c80993f4a7ac4ce&quot; &gt;&lt;/div&gt;

&lt;/body&gt;
&lt;script&gt;


            var map_7170e2328a0347478c80993f4a7ac4ce = L.map(
                &quot;map_7170e2328a0347478c80993f4a7ac4ce&quot;,
                {
                    center: [37.6109, 126.9973],
                    crs: L.CRS.EPSG3857,
                    zoom: 17,
                    zoomControl: true,
                    preferCanvas: false,
                }
            );





            var tile_layer_83b658b1d8652cce42a968efe92fc78f = L.tileLayer(
                &quot;https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png&quot;,
                {&quot;attribution&quot;: &quot;Data by \u0026copy; \u003ca target=\&quot;_blank\&quot; href=\&quot;http://openstreetmap.org\&quot;\u003eOpenStreetMap\u003c/a\u003e, under \u003ca target=\&quot;_blank\&quot; href=\&quot;http://www.openstreetmap.org/copyright\&quot;\u003eODbL\u003c/a\u003e.&quot;, &quot;detectRetina&quot;: false, &quot;maxNativeZoom&quot;: 18, &quot;maxZoom&quot;: 18, &quot;minZoom&quot;: 0, &quot;noWrap&quot;: false, &quot;opacity&quot;: 1, &quot;subdomains&quot;: &quot;abc&quot;, &quot;tms&quot;: false}
            ).addTo(map_7170e2328a0347478c80993f4a7ac4ce);


            var feature_group_ca6cd9e9ec55665ce11735c8e3871091 = L.featureGroup(
                {}
            ).addTo(map_7170e2328a0347478c80993f4a7ac4ce);


            var marker_a72fab9ec0208e648026552458f49140 = L.marker(
                [37.5137422, 127.05544630232768],
                {}
            ).addTo(feature_group_ca6cd9e9ec55665ce11735c8e3871091);


            var icon_4696c65740f7e5b7e2e5fd90bf864920 = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;landmark&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;darkblue&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_a72fab9ec0208e648026552458f49140.setIcon(icon_4696c65740f7e5b7e2e5fd90bf864920);


        var popup_5f6ed6fc55f85bac60a84dfdf84c7fa9 = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_21d75fcb5dc52630d1c2c665c9f80083 = $(`&lt;div id=&quot;html_21d75fcb5dc52630d1c2c665c9f80083&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;강남구&lt;/div&gt;`)[0];
                popup_5f6ed6fc55f85bac60a84dfdf84c7fa9.setContent(html_21d75fcb5dc52630d1c2c665c9f80083);



        marker_a72fab9ec0208e648026552458f49140.bindPopup(popup_5f6ed6fc55f85bac60a84dfdf84c7fa9)
        ;




            var marker_12e2b492327fa5cded976db4a843c248 = L.marker(
                [37.54504585, 127.09445790724516],
                {}
            ).addTo(feature_group_ca6cd9e9ec55665ce11735c8e3871091);


            var icon_b49df2c5117a97b2b7d7416f9bfaacf2 = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;landmark&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;darkblue&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_12e2b492327fa5cded976db4a843c248.setIcon(icon_b49df2c5117a97b2b7d7416f9bfaacf2);


        var popup_eeb3dcf3f57b44aea24c9549b5221393 = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_ebe1576ace1b2c35b3264d41cde97727 = $(`&lt;div id=&quot;html_ebe1576ace1b2c35b3264d41cde97727&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;광진구&lt;/div&gt;`)[0];
                popup_eeb3dcf3f57b44aea24c9549b5221393.setContent(html_ebe1576ace1b2c35b3264d41cde97727);



        marker_12e2b492327fa5cded976db4a843c248.bindPopup(popup_eeb3dcf3f57b44aea24c9549b5221393)
        ;




            var marker_bd2d4ec7f0c7f8594e2ae39f7debacc7 = L.marker(
                [37.6541, 127.0291],
                {}
            ).addTo(feature_group_ca6cd9e9ec55665ce11735c8e3871091);


            var icon_92d90d528a0ce52615210aa8241bee3d = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;landmark&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;darkblue&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_bd2d4ec7f0c7f8594e2ae39f7debacc7.setIcon(icon_92d90d528a0ce52615210aa8241bee3d);


        var popup_ada1d30db079994c812fac6840de05ef = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_772a480f2752efe6338a7cf670d00209 = $(`&lt;div id=&quot;html_772a480f2752efe6338a7cf670d00209&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;도봉구&lt;/div&gt;`)[0];
                popup_ada1d30db079994c812fac6840de05ef.setContent(html_772a480f2752efe6338a7cf670d00209);



        marker_bd2d4ec7f0c7f8594e2ae39f7debacc7.bindPopup(popup_ada1d30db079994c812fac6840de05ef)
        ;




            var marker_07f30e86525b9f742f8ffa121ed6b8d5 = L.marker(
                [37.5558809, 126.9062727],
                {}
            ).addTo(feature_group_ca6cd9e9ec55665ce11735c8e3871091);


            var icon_108c26f86677cc1063ea4558705f4d90 = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;landmark&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;darkblue&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_07f30e86525b9f742f8ffa121ed6b8d5.setIcon(icon_108c26f86677cc1063ea4558705f4d90);


        var popup_f2f87b7d275e51aa8a06f57c674ca1b6 = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_e521a2cc143ab78032b40f2fd155771a = $(`&lt;div id=&quot;html_e521a2cc143ab78032b40f2fd155771a&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;마포구&lt;/div&gt;`)[0];
                popup_f2f87b7d275e51aa8a06f57c674ca1b6.setContent(html_e521a2cc143ab78032b40f2fd155771a);



        marker_07f30e86525b9f742f8ffa121ed6b8d5.bindPopup(popup_f2f87b7d275e51aa8a06f57c674ca1b6)
        ;




            var marker_5f38fab387790c98618562a5c86bcb5a = L.marker(
                [37.5497, 126.9456],
                {}
            ).addTo(feature_group_ca6cd9e9ec55665ce11735c8e3871091);


            var icon_4793d2bcd96f7f5c2f19e8f353cd7708 = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;landmark&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;darkblue&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_5f38fab387790c98618562a5c86bcb5a.setIcon(icon_4793d2bcd96f7f5c2f19e8f353cd7708);


        var popup_b9e7d4a8b865b466ad6b66491f42456d = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_a8f40ee643fc9e4759df538909d77467 = $(`&lt;div id=&quot;html_a8f40ee643fc9e4759df538909d77467&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;마포아트센터&lt;/div&gt;`)[0];
                popup_b9e7d4a8b865b466ad6b66491f42456d.setContent(html_a8f40ee643fc9e4759df538909d77467);



        marker_5f38fab387790c98618562a5c86bcb5a.bindPopup(popup_b9e7d4a8b865b466ad6b66491f42456d)
        ;




            var marker_b6c05bf4bfad4d60c2ff0af008ae8f2b = L.marker(
                [37.5937788, 126.9497219],
                {}
            ).addTo(feature_group_ca6cd9e9ec55665ce11735c8e3871091);


            var icon_dad23ddfc9f0360706ff3903495fcef6 = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;landmark&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;darkblue&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_b6c05bf4bfad4d60c2ff0af008ae8f2b.setIcon(icon_dad23ddfc9f0360706ff3903495fcef6);


        var popup_3ba66754aff6e8a662f395d8ff7d9dae = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_ab365c5a55b0c87ae511220f047e791d = $(`&lt;div id=&quot;html_ab365c5a55b0c87ae511220f047e791d&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;서대문구&lt;/div&gt;`)[0];
                popup_3ba66754aff6e8a662f395d8ff7d9dae.setContent(html_ab365c5a55b0c87ae511220f047e791d);



        marker_b6c05bf4bfad4d60c2ff0af008ae8f2b.bindPopup(popup_3ba66754aff6e8a662f395d8ff7d9dae)
        ;




            var marker_591844d26eba07bcba3a3ab28a9c4ed3 = L.marker(
                [37.5217, 127.1244],
                {}
            ).addTo(feature_group_ca6cd9e9ec55665ce11735c8e3871091);


            var icon_12cb3fb7878de87ec0a4d6230ddfa3e1 = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;landmark&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;darkblue&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_591844d26eba07bcba3a3ab28a9c4ed3.setIcon(icon_12cb3fb7878de87ec0a4d6230ddfa3e1);


        var popup_c39e5b5d062cea905f279e093c656218 = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_b9743afe191e198d96d72bcba96a5dea = $(`&lt;div id=&quot;html_b9743afe191e198d96d72bcba96a5dea&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;올림픽공원&lt;/div&gt;`)[0];
                popup_c39e5b5d062cea905f279e093c656218.setContent(html_b9743afe191e198d96d72bcba96a5dea);



        marker_591844d26eba07bcba3a3ab28a9c4ed3.bindPopup(popup_c39e5b5d062cea905f279e093c656218)
        ;




            var marker_3a1e43ba5ce1107e575762ed027230b0 = L.marker(
                [37.608413, 126.9291524],
                {}
            ).addTo(feature_group_ca6cd9e9ec55665ce11735c8e3871091);


            var icon_8698748f48c83a7f9bf8b2bb40256b37 = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;landmark&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;darkblue&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_3a1e43ba5ce1107e575762ed027230b0.setIcon(icon_8698748f48c83a7f9bf8b2bb40256b37);


        var popup_3df2b11ab6c1dd105fb2bf24bbf07644 = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_0cef4c1d8bc7a24a23221508df64bc5f = $(`&lt;div id=&quot;html_0cef4c1d8bc7a24a23221508df64bc5f&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;은평구&lt;/div&gt;`)[0];
                popup_3df2b11ab6c1dd105fb2bf24bbf07644.setContent(html_0cef4c1d8bc7a24a23221508df64bc5f);



        marker_3a1e43ba5ce1107e575762ed027230b0.bindPopup(popup_3df2b11ab6c1dd105fb2bf24bbf07644)
        ;




            var marker_7b95f53b10c00d140f2b00bb2110e101 = L.marker(
                [37.5767, 126.9378],
                {}
            ).addTo(feature_group_ca6cd9e9ec55665ce11735c8e3871091);


            var icon_63fce9886ad1523df0de0db1615f9fa7 = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;landmark&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;darkblue&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_7b95f53b10c00d140f2b00bb2110e101.setIcon(icon_63fce9886ad1523df0de0db1615f9fa7);


        var popup_70598bb23380e9bc4722729d6f26efbf = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_5a05e23825d193cb8bd6d5157a0afaef = $(`&lt;div id=&quot;html_5a05e23825d193cb8bd6d5157a0afaef&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;자연사박물관&lt;/div&gt;`)[0];
                popup_70598bb23380e9bc4722729d6f26efbf.setContent(html_5a05e23825d193cb8bd6d5157a0afaef);



        marker_7b95f53b10c00d140f2b00bb2110e101.bindPopup(popup_70598bb23380e9bc4722729d6f26efbf)
        ;




            var feature_group_fd64d43011b008bbcfef4c019bda6b2c = L.featureGroup(
                {}
            ).addTo(map_7170e2328a0347478c80993f4a7ac4ce);


            var marker_eb284c3d1eff0868294d29a3b8869996 = L.marker(
                [37.47789, 127.0385291],
                {}
            ).addTo(feature_group_fd64d43011b008bbcfef4c019bda6b2c);


            var icon_3d71a2d215094f46cbc04d2a19cbaf1a = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;user&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;purple&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_eb284c3d1eff0868294d29a3b8869996.setIcon(icon_3d71a2d215094f46cbc04d2a19cbaf1a);


        var popup_8acaaf74a64dc7ffafdace50cf46ea12 = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_43c33b2ab82e6335201ad15df347299e = $(`&lt;div id=&quot;html_43c33b2ab82e6335201ad15df347299e&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;강남대로&lt;/div&gt;`)[0];
                popup_8acaaf74a64dc7ffafdace50cf46ea12.setContent(html_43c33b2ab82e6335201ad15df347299e);



        marker_eb284c3d1eff0868294d29a3b8869996.bindPopup(popup_8acaaf74a64dc7ffafdace50cf46ea12)
        ;




            var marker_048894e49fc659c15cb0ccccd31e47ae = L.marker(
                [37.5434959, 127.0270878],
                {}
            ).addTo(feature_group_fd64d43011b008bbcfef4c019bda6b2c);


            var icon_95389a65fb039c9dc71b4d33b55136dd = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;user&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;purple&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_048894e49fc659c15cb0ccccd31e47ae.setIcon(icon_95389a65fb039c9dc71b4d33b55136dd);


        var popup_df02b3b9a528c9694c3677905984eab0 = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_1966f47f9429a2f8ecee908a05e528d4 = $(`&lt;div id=&quot;html_1966f47f9429a2f8ecee908a05e528d4&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;강변북로&lt;/div&gt;`)[0];
                popup_df02b3b9a528c9694c3677905984eab0.setContent(html_1966f47f9429a2f8ecee908a05e528d4);



        marker_048894e49fc659c15cb0ccccd31e47ae.bindPopup(popup_df02b3b9a528c9694c3677905984eab0)
        ;




            var marker_48a3b917bcc5f21b258f6c0a5f1021ce = L.marker(
                [37.6479732, 127.0115593],
                {}
            ).addTo(feature_group_fd64d43011b008bbcfef4c019bda6b2c);


            var icon_f8f5213b300fefbed1c08d9f7c9b89fd = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;user&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;purple&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_48a3b917bcc5f21b258f6c0a5f1021ce.setIcon(icon_f8f5213b300fefbed1c08d9f7c9b89fd);


        var popup_152ad6cbbd8ce7a44cb1ffa0c5f8ab3e = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_24b5eb20e35778bb3be8b43d54612c14 = $(`&lt;div id=&quot;html_24b5eb20e35778bb3be8b43d54612c14&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;강북구&lt;/div&gt;`)[0];
                popup_152ad6cbbd8ce7a44cb1ffa0c5f8ab3e.setContent(html_24b5eb20e35778bb3be8b43d54612c14);



        marker_48a3b917bcc5f21b258f6c0a5f1021ce.bindPopup(popup_152ad6cbbd8ce7a44cb1ffa0c5f8ab3e)
        ;




            var marker_b1d336d8afd239128648e505ff4f5085 = L.marker(
                [37.54466255, 126.83519224834023],
                {}
            ).addTo(feature_group_fd64d43011b008bbcfef4c019bda6b2c);


            var icon_63749fcb5a30405f356f926c4d982385 = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;user&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;purple&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_b1d336d8afd239128648e505ff4f5085.setIcon(icon_63749fcb5a30405f356f926c4d982385);


        var popup_cd3f5dda02139064a476b46cd6b00c9e = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_820055bd5ac0b5fde3f1ecf888300ea6 = $(`&lt;div id=&quot;html_820055bd5ac0b5fde3f1ecf888300ea6&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;강서구&lt;/div&gt;`)[0];
                popup_cd3f5dda02139064a476b46cd6b00c9e.setContent(html_820055bd5ac0b5fde3f1ecf888300ea6);



        marker_b1d336d8afd239128648e505ff4f5085.bindPopup(popup_cd3f5dda02139064a476b46cd6b00c9e)
        ;




            var marker_249ee0ad564fa16f2eb3f6ea0cf0b34d = L.marker(
                [37.4983791, 126.8909735],
                {}
            ).addTo(feature_group_fd64d43011b008bbcfef4c019bda6b2c);


            var icon_8b89f073f9a45600350b999493c8d5be = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;user&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;purple&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_249ee0ad564fa16f2eb3f6ea0cf0b34d.setIcon(icon_8b89f073f9a45600350b999493c8d5be);


        var popup_512feb60e79b87b4cf221855cdb72e1c = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_c46ca692140c341846a7e52b7a728ccd = $(`&lt;div id=&quot;html_c46ca692140c341846a7e52b7a728ccd&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;구로구&lt;/div&gt;`)[0];
                popup_512feb60e79b87b4cf221855cdb72e1c.setContent(html_c46ca692140c341846a7e52b7a728ccd);



        marker_249ee0ad564fa16f2eb3f6ea0cf0b34d.bindPopup(popup_512feb60e79b87b4cf221855cdb72e1c)
        ;




            var marker_c0b332227b28cb20fcfc2f76cb1e95ae = L.marker(
                [37.52232055, 127.02769395302175],
                {}
            ).addTo(feature_group_fd64d43011b008bbcfef4c019bda6b2c);


            var icon_44325dc34efbaee9f254502340768279 = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;user&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;purple&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_c0b332227b28cb20fcfc2f76cb1e95ae.setIcon(icon_44325dc34efbaee9f254502340768279);


        var popup_b9b6c1e589bbcbb5746324ba4c1ab485 = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_0f779bb961d83dfef1d75600162d4e49 = $(`&lt;div id=&quot;html_0f779bb961d83dfef1d75600162d4e49&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;도산대로&lt;/div&gt;`)[0];
                popup_b9b6c1e589bbcbb5746324ba4c1ab485.setContent(html_0f779bb961d83dfef1d75600162d4e49);



        marker_c0b332227b28cb20fcfc2f76cb1e95ae.bindPopup(popup_b9b6c1e589bbcbb5746324ba4c1ab485)
        ;




            var marker_ae9531e808bf2d53d7c06e2cc569c29e = L.marker(
                [37.4809533, 126.9716647],
                {}
            ).addTo(feature_group_fd64d43011b008bbcfef4c019bda6b2c);


            var icon_f86881fb7dcb9f262ac37b434bbdc77c = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;user&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;purple&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_ae9531e808bf2d53d7c06e2cc569c29e.setIcon(icon_f86881fb7dcb9f262ac37b434bbdc77c);


        var popup_29717aca92dbf7b74736781b16aa4667 = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_02f92d1e5856314da958ab0dca7afc55 = $(`&lt;div id=&quot;html_02f92d1e5856314da958ab0dca7afc55&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;동작구&lt;/div&gt;`)[0];
                popup_29717aca92dbf7b74736781b16aa4667.setContent(html_02f92d1e5856314da958ab0dca7afc55);



        marker_ae9531e808bf2d53d7c06e2cc569c29e.bindPopup(popup_29717aca92dbf7b74736781b16aa4667)
        ;




            var marker_2cff0b3aafb796b52285e4ceb224c772 = L.marker(
                [37.504572550000006, 126.99451027582955],
                {}
            ).addTo(feature_group_fd64d43011b008bbcfef4c019bda6b2c);


            var icon_2b117fecf4ac90cd48108bf194470290 = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;user&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;purple&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_2cff0b3aafb796b52285e4ceb224c772.setIcon(icon_2b117fecf4ac90cd48108bf194470290);


        var popup_3b745350a941411a0c202055f327fa99 = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_078c677cab9c3badddd9bd4258938762 = $(`&lt;div id=&quot;html_078c677cab9c3badddd9bd4258938762&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;서초구&lt;/div&gt;`)[0];
                popup_3b745350a941411a0c202055f327fa99.setContent(html_078c677cab9c3badddd9bd4258938762);



        marker_2cff0b3aafb796b52285e4ceb224c772.bindPopup(popup_3b745350a941411a0c202055f327fa99)
        ;




            var marker_effe659f9f669fe4f070b29e0b7617d6 = L.marker(
                [37.464, 127.109],
                {}
            ).addTo(feature_group_fd64d43011b008bbcfef4c019bda6b2c);


            var icon_8ecca922bb9fb1a0db268cea327bee25 = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;user&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;purple&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_effe659f9f669fe4f070b29e0b7617d6.setIcon(icon_8ecca922bb9fb1a0db268cea327bee25);


        var popup_1658dce55627fc41b7f28dc09639e294 = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_cf957a5f60af9a7be6db403536d3f974 = $(`&lt;div id=&quot;html_cf957a5f60af9a7be6db403536d3f974&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;세곡&lt;/div&gt;`)[0];
                popup_1658dce55627fc41b7f28dc09639e294.setContent(html_cf957a5f60af9a7be6db403536d3f974);



        marker_effe659f9f669fe4f070b29e0b7617d6.bindPopup(popup_1658dce55627fc41b7f28dc09639e294)
        ;




            var marker_b39e98983f7d7d9c7ae1842a3637b007 = L.marker(
                [37.5549, 126.9362],
                {}
            ).addTo(feature_group_fd64d43011b008bbcfef4c019bda6b2c);


            var icon_c820bc9e976f6eb8dd919adc9cea9087 = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;user&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;purple&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_b39e98983f7d7d9c7ae1842a3637b007.setIcon(icon_c820bc9e976f6eb8dd919adc9cea9087);


        var popup_91d651658eb1dc8ad46dbc0ce36713de = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_2b4d751032af19776be4784dbccb769c = $(`&lt;div id=&quot;html_2b4d751032af19776be4784dbccb769c&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;신촌로&lt;/div&gt;`)[0];
                popup_91d651658eb1dc8ad46dbc0ce36713de.setContent(html_2b4d751032af19776be4784dbccb769c);



        marker_b39e98983f7d7d9c7ae1842a3637b007.bindPopup(popup_91d651658eb1dc8ad46dbc0ce36713de)
        ;




            var marker_efea33a8f6b7d212c2c8dba3a74bdca5 = L.marker(
                [37.525828, 126.8546309],
                {}
            ).addTo(feature_group_fd64d43011b008bbcfef4c019bda6b2c);


            var icon_04911c71154ea706f03987e9e452e507 = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;user&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;purple&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_efea33a8f6b7d212c2c8dba3a74bdca5.setIcon(icon_04911c71154ea706f03987e9e452e507);


        var popup_aa98137bdf4113712d4bdc76689a73e9 = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_47cde9029d57aa96f10bff1d6df5de55 = $(`&lt;div id=&quot;html_47cde9029d57aa96f10bff1d6df5de55&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;양천구&lt;/div&gt;`)[0];
                popup_aa98137bdf4113712d4bdc76689a73e9.setContent(html_47cde9029d57aa96f10bff1d6df5de55);



        marker_efea33a8f6b7d212c2c8dba3a74bdca5.bindPopup(popup_aa98137bdf4113712d4bdc76689a73e9)
        ;




            var marker_67a321e0f94ae3c22084453bd9bd465e = L.marker(
                [37.5204548, 126.9048814],
                {}
            ).addTo(feature_group_fd64d43011b008bbcfef4c019bda6b2c);


            var icon_47e7b5a868350468ec40a7a764cc5b07 = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;user&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;purple&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_67a321e0f94ae3c22084453bd9bd465e.setIcon(icon_47e7b5a868350468ec40a7a764cc5b07);


        var popup_2670b43a3be5afedc18b93b85bb3a0d5 = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_008a2b183b60586919b027377fa20e6e = $(`&lt;div id=&quot;html_008a2b183b60586919b027377fa20e6e&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;영등포로&lt;/div&gt;`)[0];
                popup_2670b43a3be5afedc18b93b85bb3a0d5.setContent(html_008a2b183b60586919b027377fa20e6e);



        marker_67a321e0f94ae3c22084453bd9bd465e.bindPopup(popup_2670b43a3be5afedc18b93b85bb3a0d5)
        ;




            var marker_684333663e3a87334ef052584f8fd40c = L.marker(
                [37.5721866, 127.0125717],
                {}
            ).addTo(feature_group_fd64d43011b008bbcfef4c019bda6b2c);


            var icon_19bb8742c41d9a66c5fca9120f8950b4 = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;user&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;purple&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_684333663e3a87334ef052584f8fd40c.setIcon(icon_19bb8742c41d9a66c5fca9120f8950b4);


        var popup_9cd9c36e7be5cd59cd613b86be024e80 = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_a9d4b8ce0dc8836834b4a516ed04877f = $(`&lt;div id=&quot;html_a9d4b8ce0dc8836834b4a516ed04877f&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;종로&lt;/div&gt;`)[0];
                popup_9cd9c36e7be5cd59cd613b86be024e80.setContent(html_a9d4b8ce0dc8836834b4a516ed04877f);



        marker_684333663e3a87334ef052584f8fd40c.bindPopup(popup_9cd9c36e7be5cd59cd613b86be024e80)
        ;




            var marker_a35875fe6ba36aa96d15b7076a36f2af = L.marker(
                [37.5679375, 126.9876113],
                {}
            ).addTo(feature_group_fd64d43011b008bbcfef4c019bda6b2c);


            var icon_01631ce8956384b2272197ec9329072f = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;user&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;purple&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_a35875fe6ba36aa96d15b7076a36f2af.setIcon(icon_01631ce8956384b2272197ec9329072f);


        var popup_8cf05c42d1a7daf8069d298f7d69ad76 = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_d2a7d336d9a92e1c4234d50eb46f0f59 = $(`&lt;div id=&quot;html_d2a7d336d9a92e1c4234d50eb46f0f59&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;청계천로&lt;/div&gt;`)[0];
                popup_8cf05c42d1a7daf8069d298f7d69ad76.setContent(html_d2a7d336d9a92e1c4234d50eb46f0f59);



        marker_a35875fe6ba36aa96d15b7076a36f2af.bindPopup(popup_8cf05c42d1a7daf8069d298f7d69ad76)
        ;




            var feature_group_a2eb51b1b29d2154ac1f4eb35ea81690 = L.featureGroup(
                {}
            ).addTo(map_7170e2328a0347478c80993f4a7ac4ce);


            var marker_164f5a790168a1ccaf0c8619575af21a = L.marker(
                [37.5460139, 127.1357223],
                {}
            ).addTo(feature_group_a2eb51b1b29d2154ac1f4eb35ea81690);


            var icon_8aa4e8a9b82ccf5610a6dcd10ee65db1 = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;building-user&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;darkred&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_164f5a790168a1ccaf0c8619575af21a.setIcon(icon_8aa4e8a9b82ccf5610a6dcd10ee65db1);


        var popup_fdfb368052f3ad039402ab986b3b0287 = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_b6d8beaa3315039b630ec8de7f6d9e80 = $(`&lt;div id=&quot;html_b6d8beaa3315039b630ec8de7f6d9e80&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;강동구&lt;/div&gt;`)[0];
                popup_fdfb368052f3ad039402ab986b3b0287.setContent(html_b6d8beaa3315039b630ec8de7f6d9e80);



        marker_164f5a790168a1ccaf0c8619575af21a.bindPopup(popup_fdfb368052f3ad039402ab986b3b0287)
        ;




            var marker_2992c164b93fb7566a7abf28fe4227a5 = L.marker(
                [37.48740505, 126.92715331638146],
                {}
            ).addTo(feature_group_a2eb51b1b29d2154ac1f4eb35ea81690);


            var icon_dfddcb1f253ba7f91b45893ea5e6e204 = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;building-user&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;darkred&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_2992c164b93fb7566a7abf28fe4227a5.setIcon(icon_dfddcb1f253ba7f91b45893ea5e6e204);


        var popup_e8ee57e430cd21e0923df5c9fb60617f = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_30bc0140a5a177559252b7ac029e40b1 = $(`&lt;div id=&quot;html_30bc0140a5a177559252b7ac029e40b1&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;관악구&lt;/div&gt;`)[0];
                popup_e8ee57e430cd21e0923df5c9fb60617f.setContent(html_30bc0140a5a177559252b7ac029e40b1);



        marker_2992c164b93fb7566a7abf28fe4227a5.bindPopup(popup_e8ee57e430cd21e0923df5c9fb60617f)
        ;




            var marker_11883dcdb0890015da10f6ca1422a2ce = L.marker(
                [37.4489883, 126.9078558],
                {}
            ).addTo(feature_group_a2eb51b1b29d2154ac1f4eb35ea81690);


            var icon_daadcb8cc4893c7a5b2cb4223808db40 = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;building-user&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;darkred&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_11883dcdb0890015da10f6ca1422a2ce.setIcon(icon_daadcb8cc4893c7a5b2cb4223808db40);


        var popup_b16b29df0bfc6d2a81dfacd4f5058212 = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_fc1672b6ddbb8e47039d4ead624032b8 = $(`&lt;div id=&quot;html_fc1672b6ddbb8e47039d4ead624032b8&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;금천구&lt;/div&gt;`)[0];
                popup_b16b29df0bfc6d2a81dfacd4f5058212.setContent(html_fc1672b6ddbb8e47039d4ead624032b8);



        marker_11883dcdb0890015da10f6ca1422a2ce.bindPopup(popup_b16b29df0bfc6d2a81dfacd4f5058212)
        ;




            var marker_cab87ee3df8efbbb3b9fd9cb6bb76321 = L.marker(
                [37.6588045, 127.0687269],
                {}
            ).addTo(feature_group_a2eb51b1b29d2154ac1f4eb35ea81690);


            var icon_bb02a19688ddfda2ce250deed85133f9 = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;building-user&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;darkred&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_cab87ee3df8efbbb3b9fd9cb6bb76321.setIcon(icon_bb02a19688ddfda2ce250deed85133f9);


        var popup_4ed20b8ec0c0f2f27a4f0bcbc3b23558 = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_e32216f3a7f3cb9a502108c239768a9e = $(`&lt;div id=&quot;html_e32216f3a7f3cb9a502108c239768a9e&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;노원구&lt;/div&gt;`)[0];
                popup_4ed20b8ec0c0f2f27a4f0bcbc3b23558.setContent(html_e32216f3a7f3cb9a502108c239768a9e);



        marker_cab87ee3df8efbbb3b9fd9cb6bb76321.bindPopup(popup_4ed20b8ec0c0f2f27a4f0bcbc3b23558)
        ;




            var marker_7533af5b98710f60e8769018464c385e = L.marker(
                [37.5774838, 127.0298009],
                {}
            ).addTo(feature_group_a2eb51b1b29d2154ac1f4eb35ea81690);


            var icon_2e833bd8fbc47a8350e2fd7cd5bd5ebc = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;building-user&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;darkred&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_7533af5b98710f60e8769018464c385e.setIcon(icon_2e833bd8fbc47a8350e2fd7cd5bd5ebc);


        var popup_1f1ee1234e0ca89724db20e135c836e1 = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_a295bad8ec31ebfbc89144880c24adb4 = $(`&lt;div id=&quot;html_a295bad8ec31ebfbc89144880c24adb4&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;동대문구&lt;/div&gt;`)[0];
                popup_1f1ee1234e0ca89724db20e135c836e1.setContent(html_a295bad8ec31ebfbc89144880c24adb4);



        marker_7533af5b98710f60e8769018464c385e.bindPopup(popup_1f1ee1234e0ca89724db20e135c836e1)
        ;




            var marker_cfc08f6e32b87cd78712767e99866db0 = L.marker(
                [37.5429, 127.0417],
                {}
            ).addTo(feature_group_a2eb51b1b29d2154ac1f4eb35ea81690);


            var icon_75f05d7b3a27dd5c78c594285efabbdd = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;building-user&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;darkred&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_cfc08f6e32b87cd78712767e99866db0.setIcon(icon_75f05d7b3a27dd5c78c594285efabbdd);


        var popup_0425057759be6f50a25c2937fa03e2a5 = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_a0dea00ed50600887f271748c29ae1f1 = $(`&lt;div id=&quot;html_a0dea00ed50600887f271748c29ae1f1&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;서울숲&lt;/div&gt;`)[0];
                popup_0425057759be6f50a25c2937fa03e2a5.setContent(html_a0dea00ed50600887f271748c29ae1f1);



        marker_cfc08f6e32b87cd78712767e99866db0.bindPopup(popup_0425057759be6f50a25c2937fa03e2a5)
        ;




            var marker_605416787161fc563e3c347a626268bb = L.marker(
                [37.542067, 127.0496432],
                {}
            ).addTo(feature_group_a2eb51b1b29d2154ac1f4eb35ea81690);


            var icon_228a427884a3a2d0fe8821ee064d53f6 = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;building-user&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;darkred&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_605416787161fc563e3c347a626268bb.setIcon(icon_228a427884a3a2d0fe8821ee064d53f6);


        var popup_408c7af11e27761f612c40346397dc3f = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_cd2d10fed89d93d20a6dc6a99c15bd16 = $(`&lt;div id=&quot;html_cd2d10fed89d93d20a6dc6a99c15bd16&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;성동구&lt;/div&gt;`)[0];
                popup_408c7af11e27761f612c40346397dc3f.setContent(html_cd2d10fed89d93d20a6dc6a99c15bd16);



        marker_605416787161fc563e3c347a626268bb.bindPopup(popup_408c7af11e27761f612c40346397dc3f)
        ;




            var marker_11be380e1cf2c3cfcd3d6e5d5de784a9 = L.marker(
                [37.605715, 127.0251738],
                {}
            ).addTo(feature_group_a2eb51b1b29d2154ac1f4eb35ea81690);


            var icon_5c8ab012ceeb9a474553c52cf81d1c69 = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;building-user&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;darkred&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_11be380e1cf2c3cfcd3d6e5d5de784a9.setIcon(icon_5c8ab012ceeb9a474553c52cf81d1c69);


        var popup_4f0958e5d011d86f9adb91bbaa1e5f53 = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_b8fbe1b0a9ec2d92eedbce391c93cfcb = $(`&lt;div id=&quot;html_b8fbe1b0a9ec2d92eedbce391c93cfcb&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;성북구&lt;/div&gt;`)[0];
                popup_4f0958e5d011d86f9adb91bbaa1e5f53.setContent(html_b8fbe1b0a9ec2d92eedbce391c93cfcb);



        marker_11be380e1cf2c3cfcd3d6e5d5de784a9.bindPopup(popup_4f0958e5d011d86f9adb91bbaa1e5f53)
        ;




            var marker_6a674208d4f9eee3c3522b5cb08a4c53 = L.marker(
                [37.5027234, 127.0925526],
                {}
            ).addTo(feature_group_a2eb51b1b29d2154ac1f4eb35ea81690);


            var icon_bebea17497bf6f5c9affe1599bbaa0fb = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;building-user&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;darkred&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_6a674208d4f9eee3c3522b5cb08a4c53.setIcon(icon_bebea17497bf6f5c9affe1599bbaa0fb);


        var popup_f5cfc004df0fa87df6561a689dbd72ce = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_28e69ab74560b65a88527f7bffc4b617 = $(`&lt;div id=&quot;html_28e69ab74560b65a88527f7bffc4b617&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;송파구&lt;/div&gt;`)[0];
                popup_f5cfc004df0fa87df6561a689dbd72ce.setContent(html_28e69ab74560b65a88527f7bffc4b617);



        marker_6a674208d4f9eee3c3522b5cb08a4c53.bindPopup(popup_f5cfc004df0fa87df6561a689dbd72ce)
        ;




            var marker_a87bcb98182a8e14a21dc0decad1ca9b = L.marker(
                [37.5160417, 126.8943428],
                {}
            ).addTo(feature_group_a2eb51b1b29d2154ac1f4eb35ea81690);


            var icon_9c66261b3699cea96a6b7393e9d577a0 = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;building-user&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;darkred&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_a87bcb98182a8e14a21dc0decad1ca9b.setIcon(icon_9c66261b3699cea96a6b7393e9d577a0);


        var popup_d03d78f4266bbc0a290df1deebf44f6b = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_48ac79bac0075666eef2cd8106535260 = $(`&lt;div id=&quot;html_48ac79bac0075666eef2cd8106535260&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;영등포구&lt;/div&gt;`)[0];
                popup_d03d78f4266bbc0a290df1deebf44f6b.setContent(html_48ac79bac0075666eef2cd8106535260);



        marker_a87bcb98182a8e14a21dc0decad1ca9b.bindPopup(popup_d03d78f4266bbc0a290df1deebf44f6b)
        ;




            var marker_ffb56e4d81f0788771c90ff679dc6b2c = L.marker(
                [37.5356442, 127.0059256],
                {}
            ).addTo(feature_group_a2eb51b1b29d2154ac1f4eb35ea81690);


            var icon_427f87300b7f5ea8b61450232893d554 = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;building-user&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;darkred&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_ffb56e4d81f0788771c90ff679dc6b2c.setIcon(icon_427f87300b7f5ea8b61450232893d554);


        var popup_567c2d5ae6915c5b5c9a6a03f36ccf0e = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_2c36ec449211d4dcdce8dd4581b4a159 = $(`&lt;div id=&quot;html_2c36ec449211d4dcdce8dd4581b4a159&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;용산구&lt;/div&gt;`)[0];
                popup_567c2d5ae6915c5b5c9a6a03f36ccf0e.setContent(html_2c36ec449211d4dcdce8dd4581b4a159);



        marker_ffb56e4d81f0788771c90ff679dc6b2c.bindPopup(popup_567c2d5ae6915c5b5c9a6a03f36ccf0e)
        ;




            var marker_8fd7bd9677b7e1a7b316a4dd84bfb8cb = L.marker(
                [37.5720378, 127.0050662],
                {}
            ).addTo(feature_group_a2eb51b1b29d2154ac1f4eb35ea81690);


            var icon_606e6439df3bd15a7053e1ac8c160df1 = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;building-user&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;darkred&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_8fd7bd9677b7e1a7b316a4dd84bfb8cb.setIcon(icon_606e6439df3bd15a7053e1ac8c160df1);


        var popup_9eeda0cd8269fbb8204b11c98d7c5db2 = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_1e3131d6195ce3d451049e2bac12d88a = $(`&lt;div id=&quot;html_1e3131d6195ce3d451049e2bac12d88a&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;종로구&lt;/div&gt;`)[0];
                popup_9eeda0cd8269fbb8204b11c98d7c5db2.setContent(html_1e3131d6195ce3d451049e2bac12d88a);



        marker_8fd7bd9677b7e1a7b316a4dd84bfb8cb.bindPopup(popup_9eeda0cd8269fbb8204b11c98d7c5db2)
        ;




            var marker_7569e422fb6fdca69f44e26de7c5d340 = L.marker(
                [37.5643052, 126.97557231633982],
                {}
            ).addTo(feature_group_a2eb51b1b29d2154ac1f4eb35ea81690);


            var icon_96667fcf67a92d313a16e65cb6be2967 = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;building-user&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;darkred&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_7569e422fb6fdca69f44e26de7c5d340.setIcon(icon_96667fcf67a92d313a16e65cb6be2967);


        var popup_57b0e29b417d1468c73635f778b639e1 = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_8b63da23af3c544885c9da058b803007 = $(`&lt;div id=&quot;html_8b63da23af3c544885c9da058b803007&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;중구&lt;/div&gt;`)[0];
                popup_57b0e29b417d1468c73635f778b639e1.setContent(html_8b63da23af3c544885c9da058b803007);



        marker_7569e422fb6fdca69f44e26de7c5d340.bindPopup(popup_57b0e29b417d1468c73635f778b639e1)
        ;




            var marker_9f9ba6f7842aea1a15e876dd183429c7 = L.marker(
                [37.5776013, 127.0902935],
                {}
            ).addTo(feature_group_a2eb51b1b29d2154ac1f4eb35ea81690);


            var icon_3d567ed57a82c18cd000a2e39b7c72b5 = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;building-user&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;darkred&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_9f9ba6f7842aea1a15e876dd183429c7.setIcon(icon_3d567ed57a82c18cd000a2e39b7c72b5);


        var popup_b763fbb09fe7dfe077969f74d30b1718 = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_39c03b2859b6b7ca2da12e2f9cb8d393 = $(`&lt;div id=&quot;html_39c03b2859b6b7ca2da12e2f9cb8d393&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;중랑구&lt;/div&gt;`)[0];
                popup_b763fbb09fe7dfe077969f74d30b1718.setContent(html_39c03b2859b6b7ca2da12e2f9cb8d393);



        marker_9f9ba6f7842aea1a15e876dd183429c7.bindPopup(popup_b763fbb09fe7dfe077969f74d30b1718)
        ;




            var marker_ad8e9c8d9548e3be971bb417295dd082 = L.marker(
                [37.6041468, 126.7837576],
                {}
            ).addTo(feature_group_a2eb51b1b29d2154ac1f4eb35ea81690);


            var icon_16468ed1fee008f6dfd13505527163fe = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;building-user&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;darkred&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_ad8e9c8d9548e3be971bb417295dd082.setIcon(icon_16468ed1fee008f6dfd13505527163fe);


        var popup_da6e819ca757ee633457ab51a4b1bf88 = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_9a6a8c9bba55f253bb851c2668e4d695 = $(`&lt;div id=&quot;html_9a6a8c9bba55f253bb851c2668e4d695&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;행주&lt;/div&gt;`)[0];
                popup_da6e819ca757ee633457ab51a4b1bf88.setContent(html_9a6a8c9bba55f253bb851c2668e4d695);



        marker_ad8e9c8d9548e3be971bb417295dd082.bindPopup(popup_da6e819ca757ee633457ab51a4b1bf88)
        ;




            var marker_3e5acbe3bfc1296d79d5276751efc585 = L.marker(
                [37.5804829, 127.04464352020752],
                {}
            ).addTo(feature_group_a2eb51b1b29d2154ac1f4eb35ea81690);


            var icon_8780981905292ab96db50c99e77f0556 = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;building-user&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;darkred&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_3e5acbe3bfc1296d79d5276751efc585.setIcon(icon_8780981905292ab96db50c99e77f0556);


        var popup_baa63272d0d63ff5b85081ec29a69508 = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_a1d14c64d569dcd37d21c2ee09944a1d = $(`&lt;div id=&quot;html_a1d14c64d569dcd37d21c2ee09944a1d&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;홍릉로&lt;/div&gt;`)[0];
                popup_baa63272d0d63ff5b85081ec29a69508.setContent(html_a1d14c64d569dcd37d21c2ee09944a1d);



        marker_3e5acbe3bfc1296d79d5276751efc585.bindPopup(popup_baa63272d0d63ff5b85081ec29a69508)
        ;




            var feature_group_7e86e3e10ded031939112ee5d869ac3f = L.featureGroup(
                {}
            ).addTo(map_7170e2328a0347478c80993f4a7ac4ce);


            var marker_9c051e47c60a1bd2ee0cfcadd77ae5ca = L.marker(
                [37.5596278, 126.8275298],
                {}
            ).addTo(feature_group_7e86e3e10ded031939112ee5d869ac3f);


            var icon_4994f5d229479f36dbcde4dea981032e = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;car-side&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;cadetblue&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_9c051e47c60a1bd2ee0cfcadd77ae5ca.setIcon(icon_4994f5d229479f36dbcde4dea981032e);


        var popup_090690aafe66b8e4ca078d773604d937 = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_3c17cdce717a6886877e74edf888d211 = $(`&lt;div id=&quot;html_3c17cdce717a6886877e74edf888d211&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;공항대로&lt;/div&gt;`)[0];
                popup_090690aafe66b8e4ca078d773604d937.setContent(html_3c17cdce717a6886877e74edf888d211);



        marker_9c051e47c60a1bd2ee0cfcadd77ae5ca.bindPopup(popup_090690aafe66b8e4ca078d773604d937)
        ;




            var marker_be0d45e90fe036d760e847953563b348 = L.marker(
                [37.4843889, 126.9718021],
                {}
            ).addTo(feature_group_7e86e3e10ded031939112ee5d869ac3f);


            var icon_98badd6195bb7c95b5b5aed0bfcc7eda = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;car-side&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;cadetblue&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_be0d45e90fe036d760e847953563b348.setIcon(icon_98badd6195bb7c95b5b5aed0bfcc7eda);


        var popup_a339c77bf54a25976368edefc1c0bf5e = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_5684c8c37294d8bc9d9233201c3d5804 = $(`&lt;div id=&quot;html_5684c8c37294d8bc9d9233201c3d5804&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;동작대로&lt;/div&gt;`)[0];
                popup_a339c77bf54a25976368edefc1c0bf5e.setContent(html_5684c8c37294d8bc9d9233201c3d5804);



        marker_be0d45e90fe036d760e847953563b348.bindPopup(popup_a339c77bf54a25976368edefc1c0bf5e)
        ;




            var marker_5eabc05ddfdc8d2bd2d41a693f6e7923 = L.marker(
                [37.599422950000005, 127.02378612455342],
                {}
            ).addTo(feature_group_7e86e3e10ded031939112ee5d869ac3f);


            var icon_dea4870ad5ee5d3ffc00b2846f282193 = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;car-side&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;cadetblue&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_5eabc05ddfdc8d2bd2d41a693f6e7923.setIcon(icon_dea4870ad5ee5d3ffc00b2846f282193);


        var popup_b82e21ac02a428a04cd1fd263fde7e3b = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_b2c4230a3168874d031daf0ffa883c08 = $(`&lt;div id=&quot;html_b2c4230a3168874d031daf0ffa883c08&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;정릉로&lt;/div&gt;`)[0];
                popup_b82e21ac02a428a04cd1fd263fde7e3b.setContent(html_b2c4230a3168874d031daf0ffa883c08);



        marker_5eabc05ddfdc8d2bd2d41a693f6e7923.bindPopup(popup_b82e21ac02a428a04cd1fd263fde7e3b)
        ;




            var marker_adf11c6846b03b597c6606f4813bb3d9 = L.marker(
                [37.6239187, 127.0907603],
                {}
            ).addTo(feature_group_7e86e3e10ded031939112ee5d869ac3f);


            var icon_3a988b0c96941ba7411e42cd9f78dfc3 = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;car-side&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;cadetblue&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_adf11c6846b03b597c6606f4813bb3d9.setIcon(icon_3a988b0c96941ba7411e42cd9f78dfc3);


        var popup_49972e97e0914001f83f3fce9797c5dd = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_32968d3a3cba3dc1ca2f911b65b0a8c3 = $(`&lt;div id=&quot;html_32968d3a3cba3dc1ca2f911b65b0a8c3&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;화랑로&lt;/div&gt;`)[0];
                popup_49972e97e0914001f83f3fce9797c5dd.setContent(html_32968d3a3cba3dc1ca2f911b65b0a8c3);



        marker_adf11c6846b03b597c6606f4813bb3d9.bindPopup(popup_49972e97e0914001f83f3fce9797c5dd)
        ;




            var feature_group_213e5f2e20ca4d38420340adcb480ee3 = L.featureGroup(
                {}
            ).addTo(map_7170e2328a0347478c80993f4a7ac4ce);


            var marker_4ac20a67e2461c135b5a0c6bbf9fe298 = L.marker(
                [37.4432, 126.9673],
                {}
            ).addTo(feature_group_213e5f2e20ca4d38420340adcb480ee3);


            var icon_85f0de7704b2374df05323d19f0455b3 = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;mountain-sun&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;green&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_4ac20a67e2461c135b5a0c6bbf9fe298.setIcon(icon_85f0de7704b2374df05323d19f0455b3);


        var popup_e577524305c6e5da9bfdcf78afa9c406 = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_4dffeb67dd7d4cd26a03c5ff54a0964b = $(`&lt;div id=&quot;html_4dffeb67dd7d4cd26a03c5ff54a0964b&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;관악산&lt;/div&gt;`)[0];
                popup_e577524305c6e5da9bfdcf78afa9c406.setContent(html_4dffeb67dd7d4cd26a03c5ff54a0964b);



        marker_4ac20a67e2461c135b5a0c6bbf9fe298.bindPopup(popup_e577524305c6e5da9bfdcf78afa9c406)
        ;




            var marker_6fd6b052d6cbac0e41b2b6084b75f66b = L.marker(
                [37.5512474, 126.98826417599432],
                {}
            ).addTo(feature_group_213e5f2e20ca4d38420340adcb480ee3);


            var icon_c3b478af1b98c81ba4c95f02ad0dfd56 = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;mountain-sun&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;green&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_6fd6b052d6cbac0e41b2b6084b75f66b.setIcon(icon_c3b478af1b98c81ba4c95f02ad0dfd56);


        var popup_c7848bdc484bb22bd473fc32f7dde625 = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_d2828921352437f9b9471ee4ce245669 = $(`&lt;div id=&quot;html_d2828921352437f9b9471ee4ce245669&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;남산&lt;/div&gt;`)[0];
                popup_c7848bdc484bb22bd473fc32f7dde625.setContent(html_d2828921352437f9b9471ee4ce245669);



        marker_6fd6b052d6cbac0e41b2b6084b75f66b.bindPopup(popup_c7848bdc484bb22bd473fc32f7dde625)
        ;




            var marker_dfaa79a2e6dd5e0ab23224b0eb00d055 = L.marker(
                [37.6789, 127.0023],
                {}
            ).addTo(feature_group_213e5f2e20ca4d38420340adcb480ee3);


            var icon_b5a77ec5af32e6faddf25aa552e41aec = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;mountain-sun&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;green&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_dfaa79a2e6dd5e0ab23224b0eb00d055.setIcon(icon_b5a77ec5af32e6faddf25aa552e41aec);


        var popup_090c50734e6e0519bc3f8695631cb8d6 = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_51d7ded6f5b0d4fd7682f31f9becd3a1 = $(`&lt;div id=&quot;html_51d7ded6f5b0d4fd7682f31f9becd3a1&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;북한산&lt;/div&gt;`)[0];
                popup_090c50734e6e0519bc3f8695631cb8d6.setContent(html_51d7ded6f5b0d4fd7682f31f9becd3a1);



        marker_dfaa79a2e6dd5e0ab23224b0eb00d055.bindPopup(popup_090c50734e6e0519bc3f8695631cb8d6)
        ;




            var feature_group_16be5a3021aef0becb37ed9819ac8a60 = L.featureGroup(
                {}
            ).addTo(map_7170e2328a0347478c80993f4a7ac4ce);


            var marker_bc2b92cfdcab982ab3ce969f5dad3231 = L.marker(
                [37.4754, 126.8993],
                {}
            ).addTo(feature_group_16be5a3021aef0becb37ed9819ac8a60);


            var icon_6c687c6e00ba556b51e706bd7c0f9dce = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;road&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;black&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_bc2b92cfdcab982ab3ce969f5dad3231.setIcon(icon_6c687c6e00ba556b51e706bd7c0f9dce);


        var popup_d3831f150aba61adc997ab2993f30cbe = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_13e14f2a8e91eea47c83f23cc526cc8c = $(`&lt;div id=&quot;html_13e14f2a8e91eea47c83f23cc526cc8c&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;시흥대로&lt;/div&gt;`)[0];
                popup_d3831f150aba61adc997ab2993f30cbe.setContent(html_13e14f2a8e91eea47c83f23cc526cc8c);



        marker_bc2b92cfdcab982ab3ce969f5dad3231.bindPopup(popup_d3831f150aba61adc997ab2993f30cbe)
        ;




            var marker_c30d5f40baa1b2deea2cf7446ed1d779 = L.marker(
                [37.5377854, 127.1265483],
                {}
            ).addTo(feature_group_16be5a3021aef0becb37ed9819ac8a60);


            var icon_a6f428d02492acfe598a5674c6d87d0a = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;road&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;black&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_c30d5f40baa1b2deea2cf7446ed1d779.setIcon(icon_a6f428d02492acfe598a5674c6d87d0a);


        var popup_2953b1e671e96e544771957ae8fc224d = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_45132d69633a614b06ad8f1dac5e5977 = $(`&lt;div id=&quot;html_45132d69633a614b06ad8f1dac5e5977&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;천호대로&lt;/div&gt;`)[0];
                popup_2953b1e671e96e544771957ae8fc224d.setContent(html_45132d69633a614b06ad8f1dac5e5977);



        marker_c30d5f40baa1b2deea2cf7446ed1d779.bindPopup(popup_2953b1e671e96e544771957ae8fc224d)
        ;




            var marker_a3789e8e5fce828116d46a13c30ccd5f = L.marker(
                [37.5545547, 126.9707793],
                {}
            ).addTo(feature_group_16be5a3021aef0becb37ed9819ac8a60);


            var icon_cc1068d99c29486e574b9c21a624196e = L.AwesomeMarkers.icon(
                {&quot;extraClasses&quot;: &quot;fa-rotate-0&quot;, &quot;icon&quot;: &quot;road&quot;, &quot;iconColor&quot;: &quot;white&quot;, &quot;markerColor&quot;: &quot;black&quot;, &quot;prefix&quot;: &quot;fa&quot;}
            );
            marker_a3789e8e5fce828116d46a13c30ccd5f.setIcon(icon_cc1068d99c29486e574b9c21a624196e);


        var popup_c241948525d968ef5c18e56cdf77fffb = L.popup({&quot;maxWidth&quot;: &quot;100%&quot;});



                var html_93456a5e34cc52defc959c82b12b696b = $(`&lt;div id=&quot;html_93456a5e34cc52defc959c82b12b696b&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;한강대로&lt;/div&gt;`)[0];
                popup_c241948525d968ef5c18e56cdf77fffb.setContent(html_93456a5e34cc52defc959c82b12b696b);



        marker_a3789e8e5fce828116d46a13c30ccd5f.bindPopup(popup_c241948525d968ef5c18e56cdf77fffb)
        ;




            var layer_control_319f795764477ae5dfc538b49dfd66d3 = {
                base_layers : {
                    &quot;openstreetmap&quot; : tile_layer_83b658b1d8652cce42a968efe92fc78f,
                },
                overlays :  {
                    &quot;cluster 0&quot; : feature_group_ca6cd9e9ec55665ce11735c8e3871091,
                    &quot;cluster 1&quot; : feature_group_fd64d43011b008bbcfef4c019bda6b2c,
                    &quot;cluster 2&quot; : feature_group_a2eb51b1b29d2154ac1f4eb35ea81690,
                    &quot;cluster 3&quot; : feature_group_7e86e3e10ded031939112ee5d869ac3f,
                    &quot;cluster 4&quot; : feature_group_213e5f2e20ca4d38420340adcb480ee3,
                    &quot;cluster 5&quot; : feature_group_16be5a3021aef0becb37ed9819ac8a60,
                },
            };
            L.control.layers(
                layer_control_319f795764477ae5dfc538b49dfd66d3.base_layers,
                layer_control_319f795764477ae5dfc538b49dfd66d3.overlays,
                {&quot;autoZIndex&quot;: true, &quot;collapsed&quot;: false, &quot;position&quot;: &quot;topright&quot;}
            ).addTo(map_7170e2328a0347478c80993f4a7ac4ce);

&lt;/script&gt;
&lt;/html&gt;" style="position:absolute;width:100%;height:100%;left:0;top:0;border:none !important;" allowfullscreen webkitallowfullscreen mozallowfullscreen></iframe></div></div>


