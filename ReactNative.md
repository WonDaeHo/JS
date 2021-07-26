# RN2021# RN2021
-----------------------------------------------------------
# 리엑트 네이티브 라이브러리 사용
2021-06-04
-----------------------------------------------------------
## 리덕스
리덕스는 "자바스크립트 앱을 위한 예측 가능한 state 컨테이너"로 정의합니다.
앱에 단 하나밖에 없는 전역 상태의 객체입니다.
이 전역 state객체는 리액트 네이티브 컴포넌트에서 props로 전달됩니다.
리덕스 state의 데이터가 변경되면, 변경된 새 데이터가 전체 앱에 props로 전달합니다.
리덕스는 앱의 state를 모두 store라는 곳으로 이동시켜 데이터 관리를 편리하게 합니다.
리덕스는 리액트의 context라는 기능을 이용해서 동작합니다.
context는 전역 state를 만들고 관리하는 매커니즘입니다.

------------------------------------------------------------------------------

## context응 이용한 전역상태 관리.
context는 전역변수를 만드는 React API입니다.
context를 전달받는 컴포넌트는 context를 만든 컴포넌트의 자식 컴포넌트라 합니다.
일반적으로 데이터를 전달하려면 컴포넌트 구조의 단계별로 props를 전달하지만 context를 이용하면 props를 사용할 필요가 없다. 
왜냐하면 전역 객체이기 때문에 앱 전체에서 context를 참조할 수 있기 때문이다.
```javascript

import React, { createContext, Component } from 'react';
import { StyleSheet, Text, View } from 'react-native';

const ThemeContext = createContext()


class App extends Component {
  state = { themeValue: 'light' }
  toggleThemeValue = () => {
    const value = this.state.themeValue === 'dark' ? 'light' : 'dark'
    this.setState({ themeValue: value })
  }
  render() {
    return (
      <ThemeContext.Provider
        value={{
          themeValue: this.state.themeValue,
          toggleThemeValue: this.toggleThemeValue
        }}
      >
        <View style={styles.container}>
          <Text>Hello World</Text>
        </View>
        <Child1 />
      </ThemeContext.Provider>
    );
  }
}

const Child1 = () => <Child2 />

const Child2 = () => (
  <ThemeContext.Consumer>
    {(val) => (
      <View style={[styles.container,
      val.themeValue === 'dark' &&
      { backgroundColor: 'black' }]}>
        <Text style={styles.text}>Hello from Component2</Text>
        <Text style={styles.text}
          onPress={val.toggleThemeValue}>
          Toggle Theme Value
          </Text>
      </View>
    )}
  </ThemeContext.Consumer>
)

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: '#F5FCFF',
  },
  text: {
    fontSize: 22,
    color: '#666'
  }
})

export default App;
```
## 리액트 네이티브 앱에 리덕스 구현하기
리덕스 관련 라이브러리 설치
```javascript
npm install redux react-redux
```
src폴더를 생성 -> 폴더에 Books.js / actions.js파일 추가 -> src폴더에 reducers폴더 생성 -> 폴더에 bookReducer.js / index.js파일 추가

# /src/reducers/index.js
```javascript
import { combineReducers } from 'redux'
import bookReducer from './bookReducer'

const rootReducer = combineReducers({
    bookReducer
})

export default rootReducer
```
# /src/reducers/bookReducer.js
```javascript
import uuidV4 from 'uuid/v4'
import { ADD_BOOK } from '../actions'

const initialState = {
    books: [{ name: 'East of Eden', author: 'John Steinbeck', id: uuidV4() }]
}

const bookReducer = (state = initialState, action) => {
    switch (action.type) {
        case ADD_BOOK:
            return {
                books: [
                    ...state.books,
                    action.book
                ]
            }
        default:
            return state
    }
}

export default bookReducer
```
# App.js
```javascript
import React from 'react'
import Books from './src/Books'
import rootReducer from './src/reducers'

import { Provider } from 'react-redux'
import { createStore } from 'redux'

const store = createStore(rootReducer)

export default class App extends React.Component {
  render() {
    return (
      <Provider store={store} >
        <Books />
      </Provider>
    )
  }
}
```
# src/Books.js
```javascript
import React from 'react'
import {
    Text,
    View,
    ScrollView,
    StyleSheet
} from 'react-native'

import { connect } from 'react-redux'

class Books extends React.Component {
    render() {
        const { books } = this.props

        return (
            <View style={styles.container}>
                <Text style={styles.title}>Books</Text>
                <ScrollView
                    keyboardShouldPersistTaps='always'
                    style={styles.booksContainer}
                >
                    {
                        books.map((book, index) => (
                            <View style={styles.book} key={index}>
                                <Text style={styles.name}>{book.name}</Text>
                                <Text style={styles.author}>{book.author}</Text>
                            </View>
                        ))
                    }
                </ScrollView>
            </View>
        )
    }
}

const styles = StyleSheet.create({
    container: {
        flex: 1
    },
    booksContainer: {
        borderTopWidth: 1,
        borderTopColor: '#ddd',
        flex: 1
    },
    title: {
        paddingTop: 30,
        paddingBottom: 20,
        fontSize: 20,
        textAlign: 'center'
    },
    book: {
        padding: 20
    },
    name: {
        fontSize: 18
    },
    author: {
        fontSize: 14,
        color: '#999'
    }
})

const mapStateToProps = (state) => ({
    books: state.bookReducer.books
})

export default connect(mapStateToProps)(Books)
```

# src/action.js
```javascript
export const ADD_BOOK = 'ADD_BOOK'
export const REMOVE_BOOK = 'REMOVE_BOOK'
import uuidV4 from 'uuid/v4'


export function addBook(book) {
    return {
        type: ADD_BOOK,
        book: {
            ...book,
            id: uuidV4()
        }
    }
}

export function removeBook(book) {
    return {
        type: REMOVE_BOOK,
        book
    }
}
```

## 도서 목록에서 도서를 삭제하는 것처럼 배열에서 항목을 제거하려면, 먼저 도서를 고유하게 식별할 수 있어야 합니다.

uuid라이브러리를 설치합니다.
```javascript
 npm install uuid
```







# 리엑트 네이티브 라이브러리 사용
2021-05-28
-----------------------------------------------------------
리엑트 네비게이션 라이브러리란?
리액트 웹 앱에서 react-route-dom과 비슷한 역할을 하는 리액트 네이비트의 리액트 네비게이션!
리액트 네이티브 앱의 네비게이션과 히스토리를 쉽게 관리할 수 있는 라이브러리중 하나이다.

## 설치
  npm install @react-navigation/native
  or
  yarn add @react-navigation/native
  
## expo로 관리하는 프로젝트에 디펜던시 추가 설치하기
  eact-native-gesture-handler react-native-reanimated react-native-screens react-native-safe-area-     context @react-native-community/masked-view
  
## React Native로 관리하는 프로젝트에 디펜던시 추가 설치하기
  react-native-reanimated react-native-gesture-handler react-native-screens react-native-safe-area-     context @react-native-community/masked-view
 
 expo란?
 expo는 리액트 네이티브를 크로스 플랫폼으로 개발하기 위한 빌드도구로써 네이티브 모듈을 보다 쉽고 편하게 사용할 수 있으며 빠르게 실제 기기에서 테스트해볼 수 있도록 해주는 XDE이다.
 
## 설치
  npm install -g expo-cli
  
## 프로젝트 생성
  expo init (프로젝트 명)
  
## 실행
  expo start

# 프로필카드 만들기
2021-05-21
-----------------------------------------------------------

## Text 컴포넌트에 스타일 적용하기

Text요소도 테두리와 배경을 갖고 margin, padding, position과 같은 레이아웃 속성을 지원.

하지만, 반대로 Text에서 사용하는 스타일을 view에서도 사용할 수는 없다.
Text에서 지원하는 대부분의 스타일을 View 엘리먼트에서는 사용할 수 없다.

Text, View 컴포넌트에 공통으로 적용되는 스타일.

## Text에 색상 적용하기
 
Text 컴포넌트의 color속성은 View컴포넌트와 동일하게 동작합니다. color 속성은 Text 컴포넌트의 텍스트
색상을 지정한다. transparent도 사용할 수 있다. text의 기본색은 검정색으로 통일.

## 프로필카드에 텍스트 추가하기.

```javascript
import React, { Component } from 'react';
import { Image, StyleSheet, Text, View} from 'react-native';    

export default class App extends Component<{}> {
  render() {
    return (
      <View style={styles.container}>
        <View style={styles.cardContainer}>
          <View style={styles.cardImageContainer}>
            <Image style={styles.cardImage}
                   source={require('./user.png')}/>
          </View>
          <View>
            <Text style={styles.cardName}>    
              John Doe
            </Text>
          </View>
          <View style={styles.cardOccupationContainer}>    
            <Text style={styles.cardOccupation}>    
              React Native Developer
            </Text>
          </View>
          <View>
            <Text style={styles.cardDescription}>    
              John is a really great JavaScript developer. He
              loves using JS to build React Native applications
              for iOS and Android.
            </Text>
          </View>
        </View>
      </View>
    );
  }
}

const profileCardColor = 'dodgerblue';

const styles = StyleSheet.create({
    container: {
        flex: 1,
        justifyContent: 'center',
        alignItems: 'center'
    },
    cardContainer: {
        alignItems: 'center',
        borderColor: 'black',
        borderWidth: 3,
        borderStyle: 'solid',
        borderRadius: 20,
        backgroundColor: profileCardColor,
        width: 300,
        height: 400
    },
    cardImageContainer: {
        alignItems: 'center',
        backgroundColor: 'white',
        borderWidth: 3,
        borderColor: 'black',
        width: 120,
        height: 120,
        borderRadius: 60,
        marginTop: 30,
        paddingTop: 15
    },
    cardImage: {
        width: 80,
        height: 80
    },
    cardName: {    
        color: 'white',
        marginTop: 30,
    },
    cardOccupationContainer: {    
        borderColor: 'black',
        borderBottomWidth: 3
    },
    cardOccupation: {    
        marginTop: 10,
        marginBottom: 10,
    },
    cardDescription: {    
        marginTop: 10,
        marginRight: 40,
        marginLeft: 40,
        marginBottom: 10
    }
});
```

## 폰트 스타일 적용하기
-------------------------------------------------------------------

## font family지정하기

fontFamily: 'monospace': IOS에서는 'monospace'옵션을 사용할 수 없으며, ios에서 사용할 경우 "Unrecognized font family 'monospace'."  오류 발생. 반면 안드로이드에서는 문제없이 사용 가능.
여러개 폰트 지정 불가.
fontFamily: 'American Typewriter', monospace': IOS에서는 "Unrecognized font family ' American Typewriter, monospace '."라는 오류 발생. 반면 안드로이드에서 지원하지 않는 폰트가 지정되면 기본 폰트를 사용.

## IOS와 안드로이드에서 모노스페이스 폰트 표시하기.
```javascript
import React, { Component } from 'react';
import { Platform, StyleSheet, Text, View} from 'react-native';    

export default class App extends Component<{}> {
    render() {
        return (
            <View style={styles.container}>
                <View style={styles.row}>
                    <CenteredText>
                        I am a monospaced font on both platforms
                    </CenteredText>
                    <BottomText>
                        {Platform.OS}    
                    </BottomText>
                </View>
            </View>
        );
    }
}

const CenteredText = (props) => (
    <Text style={[styles.centeredText, props.style]}>
        {props.children}
    </Text>
);

const BottomText = (props) => (
    <CenteredText style={[{position: 'absolute', bottom: 0},    
                          props.style]}>
        {props.children}
    </CenteredText>
);

const styles = StyleSheet.create({
    container: {
        width: 300,
        height: 300,
        margin: 40,
        marginTop: 100,
        borderWidth: 1
    },
    row: {
        alignItems: 'center',
        flex: 1,
        flexDirection: 'row',
        justifyContent: 'center'
    },
    centeredText: {
        textAlign: 'center',
        margin: 10,
        fontSize: 24,
        ...Platform.select({    
            ios: {
                fontFamily: 'American Typewriter'
            },
            android: {
                fontFamily: 'monospace'
            }
        })
    }
});
```

## fontSize속성으로 폰트 크기 조정하기

fontSize 속성은 Text 요소의 텍스트 크기를 조정한다. 기본 크기는 14px.

## 폰트 스타일 변경하기

fontStyle 속성을 이용해서 폰트의 스타일을 기울임꼴로 변경할 수 있습니다. 
기본값은 'nomal'입니다. 

## 폰트 두께 지정하기

fontWeight 속성은 폰트의 두께를 의미한다.
기본값은 'nomal'또는 '400'.
fontWeight 속성에는 'nomal', 'bold', '100~900'값을 사용할 수 있다.

## 프로필카드의 Text폰트 스타일 적용하기
```javascript
…
cardName: {
    color:  'white',
    fontWeight: 'bold',    
    fontSize: 24,    
    marginTop: 30,
},
…
cardOccupation: {
    fontWeight: 'bold',    
    marginTop: 10,
    marginBottom: 10,
},
cardDescription: {
    fontStyle: 'italic',    
    marginTop: 10,
    marginRight: 40,
    marginLeft: 40,
    marginBottom: 10
}
…
```

## 텍스트 장식하기.
-------------------------------------------------------------------

Text의 높이 지정하기.
lineheight속성은 text의 높이를 지정한다.

## 텍스트 수평정렬
textAlign속성은 요소 내 텍스트를 수평으로 어떻게 정렬될지를 지정합니다. textAlign속성에서 사용할 수 있는 옵션은 'auto', 'center', 'left', 'right', 'justify'이고 이중 'justify'는 IOS에서만 사용할 수 있다.

## 밑줄 또는 취소선 추가하기

textDecorationLine 속성을 이용해서 텍스트에 밑줄이나 취소선을 추가할 수 있다.
'none', 'underline', 'linethrough', 'underline line-through' 옵션을 사용하며,
기본값은 'none'이다. 'underline line-through' 옵션값을 사용할 때는 '' 내에 중간에 공백 문자를 이용해서
속성을 중첩으로 적용한다.

## 텍스트 장식 스타일
IOS는 선 자체의 스타일링도 지원. 안드로이드에서 선을 항상 실선.
ios에서는 textDecorationStyle을 이용해서 선의 스타일을 변경 할 수 있다.
옵션값으로 'solid', 'double', 'dotted', 'dashed'를 이용할 수 있다.
안드로이드에서는 스타일 값을 무시함.

## 텍스트에 음영(그림자)넣기
textShadowColor, textShadowOffset, textShadowRadius 속성을 이용해서 Text에 음영을 넣을 수 있습니다.
텍스트에 음영을 넣기 위해서는 다음 세가지를 지정하면 됩니다.

## 완성된 프로필
```javascript
import React, { Component } from 'react';
import { Image, StyleSheet, Text, View} from 'react-native';    

export default class App extends Component<{}> {
    render() {
        return (
            <View style={styles.container}>
                <View style={styles.cardContainer}>
                    <View style={styles.cardImageContainer}>
                        <Image style={styles.cardImage}  
                               source={require('./user.png')}/>
                    </View>
                    <View>
                        <Text style={styles.cardName}>   
                            John Doe
                        </Text>
                    </View>
                    <View style={styles.cardOccupationContainer}>    
                        <Text style={styles.cardOccupation}>   
                            React Native Developer
                        </Text>
                    </View>
                    <View>
                        <Text style={styles.cardDescription}>  
                            John is a really great JavaScript developer. 
                            He loves using JS to build React Native  
                            applications for iOS and Android.
                        </Text>
                    </View>
                </View>
            </View>
        );
    }
}

const profileCardColor = 'dodgerblue';

const styles = StyleSheet.create({
    container: {
        flex: 1,
        justifyContent: 'center',
        alignItems: 'center'
    },
    cardContainer: {
        alignItems: 'center',
        borderColor: 'black',
        borderWidth: 3,
        borderStyle: 'solid',
        borderRadius: 20,
        backgroundColor: profileCardColor,
        width: 300,
        height: 400
    },
    cardImageContainer: {
        alignItems: 'center',
        backgroundColor: 'white',
        borderWidth: 3,
        borderColor: 'black',
        width: 120,
        height: 120,
        borderRadius: 60,
        marginTop: 30,
        paddingTop: 15
    },
    cardImage: {
        width: 80,
        height: 80
    },
    cardName: {
        color: 'white',
        fontWeight: 'bold',
        fontSize: 24,
        marginTop: 30,
        textShadowColor: 'black',      
        textShadowOffset: {            
            height: 2,
            width: 2
        },
        textShadowRadius: 3            
    },
    cardOccupationContainer: { 
        borderColor: 'black',
        borderBottomWidth: 3
    },
    cardOccupation: {
        fontWeight: 'bold',
        marginTop: 10,
        marginBottom: 10,
    },
    cardDescription: {
        fontStyle: 'italic',
        marginTop: 10,
        marginRight: 40,
        marginLeft: 40,
        marginBottom: 10
    }
});
```


# 중간평가에서 주목해야 할 부분
2021-05-14
-----------------------------------------------------------

## position 이용해서 컴포넌트 배치하기

position 속성에는 relative와 absolute값을 사용할 수 있다.
리엑트 네이티브에서는 이 두 값만 사용이 가능하고, absolute 속성값으로 절대 위치를 지정하면,
top, right, bottom, left 속성도 사용할 수 있다.

```javascript

import React, { Component } from 'react';
import { StyleSheet, Text, View} from 'react-native';

export default class App extends Component<{}> {
    render() {
        return (
            <View style={styles.container}>
                <View style={styles.row}>    
                    <Example>
                        <CenteredText>A</CenteredText>
                    </Example>
                    <Example>
                        <CenteredText>B</CenteredText>
                        <View style={[styles.tinyExample,    
                                     {position: 'absolute',
                                      right: 0,
                                      bottom: 0}]}>   // 부모인 B블록 내에서 절대 위치를 사용해서 오른쪽 아래에 배치
                            <CenteredText>E</CenteredText>
                        </View>
                    </Example>
                    <Example>
                        <CenteredText>C</CenteredText>
                    </Example>
                </View>
                <Example style={{position: 'absolute',    //D블록은 부모 요소의 오른쪽 하단에 
                                 right: 0, bottom: 0}}> 
                    <CenteredText>D</CenteredText>
                </Example>
            </View>
        );
    }
}

const Example = (props) => (
    <View style={[styles.example,props.style]}>
        {props.children}
    </View>
);

const CenteredText = (props) => (
    <Text style={[styles.centeredText, props.style]}>
        {props.children}
    </Text>
);

const styles = StyleSheet.create({
    container: {
        width: 300,
        height: 300,
        margin: 40,
        marginTop: 100,
        borderWidth: 1
    },
    row: {    
        flex: 1,
        flexDirection: 'row'
    },
    example: {
        width: 100,
        height: 100,
        backgroundColor: 'grey',
        borderWidth: 1,
        justifyContent: 'center'
    },
    tinyExample: {
        width: 30,
        height: 30,
        borderWidth: 1,
        justifyContent: 'center',
        backgroundColor: 'lightgrey'
    },
    centeredText: {
        textAlign: 'center',
        margin: 10
    }
});
```



## margin과 padding 

마진 스타일을 이용해서 각 컴포넌트 사이에 위치를 상대적으로 정의할 수 있다.
리엑트 네이티브에서 마진과 패딩은 CSS와 정확하게 일치.
![image](https://user-images.githubusercontent.com/77261907/118838550-bbfd2480-b900-11eb-9e2f-66bf62b8c445.png)

margin속성에는 margin은 사방으로 top, bottom, left, right등의 속성으로 지정할 수 있다.
border와 동일하게 동작한다.
마진은 모든 컴포넌트를 예상대로 위치시킨다. 하지만 안드로이드 디바이스에서 음수 마진이 적용될 때 리액트
네이티브의 버전이 낮은 경우에는 컴포넌트가 클립핑되는 경우가 발생.
IOS와 안드로이드를 모두 지원할 계획인 경우에는 프로젝트의 처음부터 각 디바이스에서 테스트를 해보아야한다.

## 컴포넌트의 다양한 마진 적용하기.

```javascript
import React, { Component } from 'react';
import { StyleSheet, Text, View} from 'react-native';

export default class App extends Component<{}> {
    render() {
        return (
          <View style={styles.container}>
            <View style={styles.exampleContainer}>
              <Example>    
                <CenteredText>A</CenteredText>
              </Example>
            </View>
          <View style={styles.exampleContainer}>
              <Example style={{marginTop: 50}}>    //위쪽만 50
                <CenteredText>B</CenteredText>
              </Example>
          </View>
          <View style={styles.exampleContainer}>
            <Example style={{marginTop: 50, marginLeft: 10}}>    //위 50, 왼 10
              <CenteredText>C</CenteredText>
            </Example>
          </View>
          <View style={styles.exampleContainer}>
            <Example style={{marginLeft: -10, marginTop: -10}}>    //왼 -10, dnl -10
              <CenteredText>D</CenteredText>
            </Example>
          </View>
        </View>
      );
  }
}

const Example = (props) => (
    <View style={[styles.example,props.style]}>
        {props.children}
    </View>
);

const CenteredText = (props) => (
    <Text style={[styles.centeredText, props.style]}>
        {props.children}
    </Text>
);

const styles = StyleSheet.create({
    container: {
        alignItems: 'center',
        flex: 1,
        flexDirection: 'row',
        flexWrap: 'wrap',
        justifyContent: 'center',
        marginTop: 75
    },
    exampleContainer: {
        borderWidth: 1,
        width: 120,
        height: 120,
        marginLeft: 20,
        marginBottom: 20,
    },
    example: {
        width: 50,
        height: 50,
        backgroundColor: 'grey',
        borderWidth: 1,
        justifyContent: 'center'
    },
    centeredText: {
        textAlign: 'center',
        margin: 10
    }
});
```

## padding 지정하기

padding도 마진과 동일하게 사용가능하다.

```javascript
import React, { Component } from 'react';

...

       <View style={styles.container}>
        <View style={styles.exampleContainer}>
           <Example style={{}}>    
               <CenteredText>A</CenteredText>
            </Example>
        </View>
        <View style={styles.exampleContainer}>
            <Example style={{paddingTop: 50}}>    //위 50
                <CenteredText>B</CenteredText>
            </Example>
        </View>
        <View style={styles.exampleContainer}>
            <Example style={{paddingTop: 50, paddingLeft: 10}}>    //위 50, 왼 10
                <CenteredText>C</CenteredText>
            </Example>
        </View>
        <View style={styles.exampleContainer}>
            <Example style={{paddingLeft: -10, paddingTop: -10}}>    //왼 -10, 위 -10
                <CenteredText>D</CenteredText>
            </Example>
        </View>
    </View>

...

    },
    centeredText: {
       textAlign: 'center',
       margin: 10,
       borderWidth: 1,    
       backgroundColor: 'lightgrey'
    }
});
```
-----------------------------------------------------------

##border radius조합으로 다양한 도형 만들기
![image](https://user-images.githubusercontent.com/77261907/118835092-a803f380-b8fd-11eb-9548-6f7485580690.png)
```javascript
import React, { Component } from 'react';
import { StyleSheet, Text, View} from 'react-native';

export default class App extends Component<{}> {
    render() {
        return (
            <View style={styles.container}>
                <Example style={{borderRadius: 20}}>     //네 곳의 모서리가 둥근
                    <CenteredText>
                        Example 1:{"\n"}4 Rounded Corners    
                    </CenteredText>
                </Example>
                <Example style={{borderTopRightRadius: 60,  //오른쪽 두 모서리가 둥근
                                 borderBottomRightRadius: 60}}>  
                    <CenteredText>
                        Example 2:{"\n"}D Shape
                    </CenteredText>
                </Example>
                <Example style={{borderTopLeftRadius: 30, //양반대편의 모서리가 둥근 
                                 borderBottomRightRadius: 30}}>    
                    <CenteredText>
                        Example 3:{"\n"}Leaf Shape
                    </CenteredText>
                </Example>
                <Example style={{borderRadius: 60}}>    //border radius가 각 측면의 길이의 반으로 지정된 사각형.
                    <CenteredText>
                        Example 4:{"\n"}Circle
                    </CenteredText>
                </Example>
            </View>
        );
    }
}

const Example = (props) => (
    <View style={[styles.example,props.style]}>
        {props.children}
    </View>
);

const CenteredText = (props) => (    
    <Text style={[styles.centeredText, props.style]}>
        {props.children}
    </Text>
);

const styles = StyleSheet.create({
    container: {    
        flex: 1,
        flexDirection: 'row',
        flexWrap: 'wrap',
        marginTop: 75
    },
    example: {
        width: 120,
        height: 120,
        marginLeft: 20,
        marginBottom: 20,
        backgroundColor: 'grey',
        borderWidth: 2,
        justifyContent: 'center'
    },
    centeredText: {    
        textAlign: 'center',
        margin: 10
    }
});


```
##border
-----------------------------------------------------------
```javascript
import React, { Component } from 'react';
import { StyleSheet, Text, View} from 'react-native';

export default class App extends Component<{}> {
    render() {
      return (
        <View style={styles.container}>
           <Example style={{borderWidth: 1}}>    
               <Text>borderWidth: 1</Text>
           </Example>
           <Example style={{borderWidth: 3, borderLeftWidth: 0}}>    
               <Text>borderWidth: 3, borderLeftWidth: 0</Text>
           </Example>
           <Example style={{borderWidth: 3, borderLeftColor: 'red'}}>    
               <Text>borderWidth: 3, borderLeftColor: 'red'</Text>
           </Example>
           <Example style={{borderLeftWidth: 3}}>    
               <Text>borderLeftWidth: 3</Text>
           </Example>
           <Example style={{borderWidth: 1, borderStyle: 'dashed'}}>    
               <Text>borderWidth: 1, borderStyle: 'dashed'</Text>
           </Example>
         </View>
      );
    }
}

const Example = (props) => (    
    <View style={[styles.example,props.style]}>
        {props.children}
    </View>
);

const styles = StyleSheet.create({ 
    container: {
        flex: 1,
        justifyContent: 'center',
        alignItems: 'center'
    },
    example: {
        marginBottom: 15
    }
});
```
borderWidth 속성만 지정하면, 기본적으로 bordercolor속성은 'black'으로 borderStyle은 'solid'로 지정된다.
스타일에서는 세부적은 속성이 일반적인 속성보다 우선순위가 높습니다.

-----------------------------------------------------------
# 중간평가에서 주목해야 할 부분
2021-05-07
-----------------------------------------------------------

1. Class형태의 component의 선언방법.
2. 함수 형태의 component의 선언방법.
3. state 설정 및 초기화 방법.
4. props의 개념 및 전달 경로.
5. state에 초기화 된 값을 props로 전달하는 방법.
6. 일반 변수로 초기화 된 값을 props로 전달하는 방법.
7. 구조분해(비구조화) 할당을 통한 번수명 재할당.
8. 필요한 component만 import하기.
9. index.js에 App 지정하기.
익숙해 질 때 까지 반복하자.

```javascript
//MidTerm.js
  import React, { Component } from 'react'
  import DataTest from './DataTest'
  
  class Midterm extends Component{
    constructor(){
      super()
      this.state = {
      id: ,
      name: ""
      }
  }
  render() {
    const {id, name} = this.state
    return (
      <DataTest
        id={ id }
        name={ name }
        foo={[1,2,3,4,5]}
       />
      )
     }
    }
 ```
 ```javascript
 //DateTest.js
   import React, { Component } from 'react' 
   import { Text, View } from 'react-native'
//Midterm으로 부터 전달받은 props를 구조분해 할당한다.
   const DataTest = ( props ) => {
   let {id} = props
   let {name} = props
   let {foo} = props
   foo = foo.map((foom i) => {
    return <Text key={i}>{ foo }</Text>
    })
    return(
      <View> //<div>같은 느낌.
        <Text> //구조분해할당 된 변수를 이용하여 View를 구성하고, 구성된 View를 호출한 컴포넌트로 리턴해 준다.
          학번: { id }
          { '/n' }
          이름: { name }
        </Text>
        { foo }
       </View>
     )
    }
   export default DataTest //외부에서 사용할 수 있도록 export 시켜준다.
```
```javascript
//index.js
  import React from 'react'
  import {AppRegistry}, from 'react-native'
  import Midterm from './app/json'
  import {name as MyApp} from './app.json'
  AppRegistry.registerComponent(MyApp, () => Midterm);
  
  //AppRegistry.registerComponent(앱 이름, () => 시작 컴포넌트)
```
## 구조분해할당
참고자료.[https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment]

"구조분해할당" 구문은 배열이나 객체의 속겅을 해체하여, 그 값을 개별 변수에 담을 수 있게하는 JavaScript 표현식 입니다.

객체에서 변수를 재할당하는 방법

1. const foobar = {
2. foo: 1000,
3. bar: 500
4. }
foobar에 있는 foo property를 woo로 바꾸고 싶을 때 ↓↓↓
1. 구조분해 할당 없이 변수명 재할당
   const woo = foobar.foo
2. 구조분해 할당을 이용하는 방법
   const { foo:woo } = foobar
   console.log(woo) //1000
   console.log(foobar) //let basket = { foo:1000, bar:500}
3. React에서 자주 사용되는 구조분해 할당.
   this.state = {
   foo : 100,
   bar : 200
   }
   const { foo,bar } = this.state;
------------------------------------------------------------
## 배경색설정하기
![image](https://user-images.githubusercontent.com/77261907/117406404-fac9cc80-af47-11eb-82a4-8c246ae9b822.png)

-----------------------------------------------------------
# 4장(스타일)
2021-04-30
---------------------------------------------------------------

## 인라인 스타일 이용하기.
```javascript
import React, { Component } from 'react'
import { Text, View } from 'react-native'

export default class App extends Component {
  render () {
    return (
      <View style={{marginLeft: 20, marginTop: 20}}>    
        <Text style={{fontSize: 18,color: 'red'}}>Some Text</Text>    
      </View>
    )
  }
}
```
- 13번째줄: 리엑트 네이티브 컴포넌트에 인라인 스타일을 적용시킴.
- 14번째줄: 여러개의 인라인 스타일을 적용할 수 있음.
- StyleSheet에 정의된 스타일 참조하기.
```javascript
import React, { Component } from 'react'
import { StyleSheet, Text, View } from 'react-native'

export default class App extends Component {
  render () {
    return (
      <View style={styles.container}>    
        <Text style={[styles.message,styles.warning]}>Some Text</Text>    
      </View>    
    )
  }
}

const styles = StyleSheet.create({
  container: {    
    marginLeft: 20,
    marginTop: 20
  },
  message: {    
    fontSize: 18
  },
  warning: {    
    color: 'red'      
  }
});
```
- 31번째줄: styles내에 정의된 container 참조.
- 32번째줄: styles에 정의된 message와 warning 참조를 배열로
- 39~48번째줄: StyleSheet.create를 이용해서 스타일 정의. 중복된 property가 있을 때, 마지막으로 전달된 스타일이 이전 스타일을 재정의한다는 점을 기억해 두도록 합니다. 예를들어
```javascript
style={[{color: 'black'},{color: 'green'},{color: 'red'}]}
```
위 처럼 스타일 배열에 마지막으로 오는 값이 적용됨.
```javascript
style={[{color: 'black'}, styles.message]]
```
위 처럼 인라인 스타일과 스타일시트를 동시에 적용시킬 수도 있다.

-------------------------------------------------------------------------------

스타일 시트로 스타일을 관리하면 좋은점.

코드베이스만 분리해서 관리.
다른 컴포넌트에서 스타일 재사용.
개발할 때 스타일 변경이 쉬움.

-------------------------------------------------------------------------------

## 스타일 구성하는 방법

1. 컴포넌트 내에 스타일시트 선언하기
이 방법의 장점은 하나의 파일에 컴포넌트와 컴포넌트가 사용할 스타일을 완전히 캡슐화 할 수 있다. 이렇게 캡슐화된 컴포넌트는 앱 내에서 자유롭게 이동해서 사용할 수 있다.

2. 컴포넌트 파일과는 별도의 스타일 시트 선언하기.
css를 사용하는데 익숙하면, 이 방법을 따르는 것이 편할 수도 있다.
확장자 이름은 js여야 한다.

---------------------------------------------------------------------------------------

## 컴포넌트의 스타일시트를 외부로 분리하기
```javascript
import { StyleSheet } from 'react-native'

const styles = StyleSheet.create({    
  container: {    
    marginTop: 150,
    backgroundColor: '#ededed',
    flexWrap: 'wrap'
  }
})

const buttons = StyleSheet.create({    
  primary: {    
    flex: 1,
    height: 70,
    backgroundColor: 'red',
    justifyContent: 'center',
    alignItems: 'center',
    marginLeft: 20,
    marginRight: 20
  }
})

export { styles, buttons }    
```

- 96번째줄 : styles상수에 스타일 생성. 97번째줄 : container스타일을 생성하고 컴포넌트에서는 styles.container로 참조 
- 104번째줄 : 두 번째 스타일을 생성하고 button상수로 저장. 
- 105번째줄 : primary button을 위한 스타일 생성. 컴포넌트에서는 buttons.primary로 참조. 
- 116번째줄 : styles와 button 모두 export해서 외부에서 사용할 수 있도록 한다.

컴포넌트는 외부 스타일 시트를 가져와서 외부 스타일시트에 정의된 스타일을 참조할 수 있습니다.

----------------------------------------------------------------------------------------------

## 외부 스타일시트 가져오기.
```javascript
import { styles, buttons } from './component/styles'
import React, { Component } from 'react'
import { Text, View, TouchableHighlight } from 'react-native'

export default class App extends Component {
render(){
  return(
    <View style={styles.container}>    
      <TouchableHighlight style={buttons.primary} />    
        <Text>Sample Text</Text>
      </TouchableHighlight>
    </View>
    )
  }
}
```
## 컴포넌트 파일에서 사용하게 될 외부로 분리한 스타일
```javascript
import {StyleSheet} from 'react-native';

export const Colors = {    //밝은색과 어두운색 테마에 사용할 색상을 정의하는 상수
    dark: 'black',
    light: 'white'
};

const baseContainerStyles = {    //기본 컨테이너의 스타일을 저장할 자바스크립트 객체
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center'
};

const baseBoxStyles = {    //기본 상자스타일을 저장할 자바스크립트 개체
    justifyContent: 'center',
    alignItems: 'center',
    borderWidth: 2,
    height: 150,
    width: 150
};

const lightStyleSheet = StyleSheet.create({    //밝은색 테마에 사용할 스타일 시트 만들기
    container: {
        ...baseContainerStyles,
        backgroundColor: Colors.light
    },
    box: {
        ...baseBoxStyles,
        borderColor: Colors.dark
    }
});

const darkStyleSheet = StyleSheet.create({    //어두운색 테마에 사용할 시트 만들기
    container: {
        ...baseContainerStyles,
        backgroundColor: Colors.dark
    },
    box: {
        ...baseBoxStyles,
        borderColor: Colors.light
    }
});

export default function getStyleSheet(useDarkTheme){    //Boolean값에 따라 해당하는 테마를 반환하는 함수
    return useDarkTheme ? darkStyleSheet : lightStyleSheet;  
}
```

스타일을 정의했으므로, App.js에서 App컴포넌트를 만들어 보도록 한다. 밝은 색과 어두운 색 테마만 사용하므로 Boolean 값만 전달받는 getStyleSheet 함수를 만든다. 함수의 반환값이 true면 어두운색 테마가 반환되고, 함수의 반환값이 false면, 밝은색 테마가 반환될 것이다.

---------------------------------------------------------------------------

## 밝은 색과 어두운 색 테마를 토글하는 앱
```javascript
import React, { Component } from 'react';
import { Button, StyleSheet, View } from 'react-native';
import getStyleSheet from './styles';    //외부로 분리해 둔 getStyleSheet함수 가져오기

export default class App extends Component {

  constructor(props) {
      super(props);
      this.state = {
          darkTheme: false    //기본 테마 색을 밝은색으로 컴포넌트의 state 초기화하기.
      };
      this.toggleTheme = this.toggleTheme.bind(this);     //예외가 발생하지 않도록 toggleTheme함수를 컴포넌트에 bind
  }

  toggleTheme() {
      this.setState({darkTheme: !this.state.darkTheme})    /호출할 때 마다 스타일을 toogle
  };

  render() {  //표기할 테마에 적합한 스타일 시트를 가져오기 위해 getStyleSheet함수 사용.

    const styles = getStyleSheet(this.state.darkTheme);    
    const backgroundColor =
          StyleSheet.flatten(styles.container).backgroundColor;    //background Color를 쉽게 사용하려면 StyleSheet의 flatten을 이용해서 StyleSheet객체를 Javascript객체로 변환

    return (
        <View style={styles.container}>    
            <View style={styles.box}>    //프로필 카드를 수평축에서 중앙으로 정렬
                <Button title={backgroundColor}    
                        onPress={this.toggleTheme}/>    //사용중인 테마의 색상을 텍스트로 표시하고, 버튼이 클릭되면 toogleTheme호출
            </View>
        </View>
    );
  }
}
```
