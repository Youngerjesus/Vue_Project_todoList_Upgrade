--------------------theory-------------------- 
vue instance 
뷰로 화면을 개발하기 위한 필수 단위
뷰 인스턴스로 화면을 렌더링할떄는 화면이 그려질 위치의 돔을 지정해줘야한다. 
created: 뷰 인스턴스가 생성되자마자 실행할 로직을 수행하는곳 뷰 인스턴스가 업데이트될떄마다 수행되는건가? 
뷰 인스턴스를 생성하면 html 특정범위안에서만 옵션 속성들이 적용되게한다. 이걸 뷰 인스턴스의 유효범위

    인스턴스가 생성된 후 화면에 어떻게 적용되는가?
    뷰 라이브러리로 파일 로딩 -> 인스턴스 객체생성 -> 특정 화면 요소에 인스턴스 붙힘 -> 인스턴스 내용이 화면요소로 변환
    -> 변환된 요소를 사용자가 봄

 vue instance life-cycle 
    인스턴스 상태에따라서 호출할 수 있는 속성을 라이프 사이클 속성이라고 한다.
    각 라이프 사이클 속성에서 실행되는 커스텀 로직을 라이프 사이클 속성이라고한다.

    beforeCreted() : 인스턴스가 생성된 직후 data속성과 method이 아직 정의 x
    created() : data와 methods이 정의되있어서 this.data 또는 this.fetchData() 와 같은 로직이용가능
    하지만 template은 안됨 인스턴스가 화면에 붙기전이라 서버에 데이터를 요청하고 받아오기 좋음
    beforeMount : created 이후 template속성에 지정한 마크업속성을 render() 함수로 변환한 후 el속성에 지정한 화면요소에 인스턴스를 부착하기 전에 호출
    mounted : el속성에 인스턴스가 부착되고 난 후 호출하는 단계 template 속성에의한 화면 요소에 접근가능하나
    하위 컴포넌트나 외부 라이브러리에 의해 추가한 화면 요소들이 최종 html코드로 변환된 시점과는 다를 수 있다
    변환되는 시점이 다른경우 $nextTick()을통해 기다렸다가 실행
    beforeUpdated : $watch속성으로 속성들을 감시한다. 관찰하고 있는 데이터가 변한다면 가상 돔으로 화면을 그리기전에 호출되는 단계
    변경예정인 새 데이터에 접근가능
    Updated : 데이터가 변경되고나서 가상돔으로 화면을 그리고나면 실행하는 단계 데이터가 변경되지않느다면 실행하지않네 . watch에의해 감시하니까 
    destoryed : 뷰 인스턴스가 파괴되고나서 호출되는 단계 하위 컴포넌트까지 다 파괴
커스텀 로직
    개발자가 임의로 작성한 추가 로직

rendering 
렌더링은 컴퓨터 프로그램을 사용하여 모델로부터 영상을 만들어내는 과정을 말한다. 하나의 씬 파일에는 정확히 정의된 언어나 자료 구조로 이루어진 개체들이 있으며, 여기에는 가상의 장면을 표현하는 도형의 배열, 시점, 텍스처 매핑, 조명, 셰이딩 정보가 포함될 수 있다

뷰 컴포넌트
    전역 컴포넌트 vs 지역 컴포넌트
    전역 컴포넌트는 한번 등록하면 어느 인스턴스든지 tag를 이용해서 사용가능 지역 컴포넌트는 매번 인스턴스에 등록을 해줘야한다.
    지역 컴포넌트에서는 서로의 데이터를 참조 할 수 없다  
    상위 컴포넌트에서 하위 컴포넌트로 데이터를 줄려면 props를 통해서 하위에서 상위로 줄려면 이벤트를 통해서 준다

    이벤트 버스를 통해서 컴포넌트간의 데이터 전달을 유연하게 가능
    컴포넌트간의 수직관계를 안따져도 되는거지. 

뷰 라우팅


--------------------implementation--------------------
index.html
    <meta name="viewport" content="width=device-width, user-scalable=no">
    width 속성은 뷰포트의 크기를 조정한다.
    initial-scale 속성은 페이지가 처음 로드될 때 줌 레벨을 조정한다. maximum-scale, minimum-scale, 그리고 user-scalable 속성들은 사용자가 얼마나 페이지를 줌-인, 줌-아우트 할 수 있는지를 조정한다.

    viewport
    뷰포트를 대략 정의하자면,
    컴퓨터나 휴대 단말기 등 장치에 Display 요소가 표현되는 영역을 말한다
    가상 "window"상에 페이지를 렌더링하는곳
    뷰포트 메타태그를 이용하면 모바일 브라우저의 뷰포트 크기나 배율등을 조정할 수 있어
    모바일 웹에 최적의 상태로 화면이 표시되도록 설정할 수 있게 된다

    https://developer.apple.com/safari/resources/#//apple_ref/doc/uid/TP40006509-SW1
    https://developer.apple.com/safari/resources/

    1) m.naver.com
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0, user-scalable=no, target-densitydpi=medium-dpi" />

    2) m.daum.net
    <meta name="viewport" content="user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0, width=device-width" />

    3) m.nate.com
    <meta name="viewport" content="user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0, width=device-width" />

    4) m.yahoo.com
    
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0,     user-scalable=no" / >

    5) m.aladdin.co.kr
    
  <meta name="viewport" content="user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0, width=device-width" /
 
  <link rel="shortcut icon" href="src/assets/favicon.ico" type="image/x-icon">
    linke rel
        rel은 일단 태그아아니라 속성을 말한다. relation의 줄입말
        <link rel="alternate" type="application/rss+xml" title="[##_title_##]" href="[##_rss_url_##]" />
  <link rel="stylesheet" media="screen" type="text/css" href="./style.css" />
  <link rel="stylesheet" media="print" type="text/css" href="./images/print.css" />
  <link rel="shortcut icon" href="[##_blog_link_##]favicon.ico" />
        <link rel="stylesheet" media="screen" type="text/css" href="./style.css" />
        '스타일시트'로 '모니터 화면에 보여줄' '텍스트 css'로 이루어진' 'style.css'파일을 이 html과 '연결'하라는 이야기죠.

    favicon.ico
    favorite + icon 의 합성어를 말한다 웹페이지를 대표하는 아이콘을 말한다.