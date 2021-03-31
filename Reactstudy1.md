시간을 시, 분 ,초 ,밀리세컨드까지 표시하도록 하는 코드
```react
import React from "react";
import ReactDOM from "react-dom";

class MyClock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      time: new Date()
    };
  }
  componentDidMount() {
    this.interval = setInterval(() => {
      this.setState({
        time: new Date()
      });
    }, 1);
  }
  render() {
    const { time } = this.state;
    const h = time.getHours();
    const m = time.getMinutes();
    const s = time.getSeconds();
    const ms = time.getMilliseconds();
    return (
      <div>
        <h1>
          {h}:{m < 10 ? "0" + m : m}:{s < 10 ? "0" + s : s}.{ms}//밀리세컨드를 표기할 때는 :가 아닌 .으로 표기
        </h1>
      </div>
    );
  }
}
ReactDOM.render(<MyClock />, document.querySelector("#app"));
```
