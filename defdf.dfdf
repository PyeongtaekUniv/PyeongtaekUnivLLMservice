"use client";

import React, { useState, useRef, useEffect } from "react";
import styles from "../styles";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faPaperPlane, faSpinner } from "@fortawesome/free-solid-svg-icons";

const Chatbot = () => {
  const [isLoading, setIsLoading] = useState(false);

  const textAreaRef = useRef(null);

  const [messages, setMessages] = useState([]);
  const [input, setInput] = useState("");

  const messagesEndRef = useRef(null);

  useEffect(() => {
    messagesEndRef.current?.scrollIntoView({ behavior: "smooth" });
  }, [messages]);

  const handleInputChange = (e) => {
    setInput(e.target.value);
    const textArea = textAreaRef.current;
    textArea.style.height = "inherit"; // 높이 초기화
    textArea.style.height = `${textArea.scrollHeight}px`; // 내용물에 맞게 높이 조정
  };

  const handleKeyPress = (e) => {
    if (e.key === "Enter" && !e.shiftKey) {
      e.preventDefault(); // Enter가 눌렸을 때의 기본 이벤트 방지
      sendMessage();
    }
  };

  const sendMessage = async () => {
    if (input.trim() === "") return;

    const newMessage = { text: input, type: "user" };
    setMessages((prevMessages) => [...prevMessages, newMessage]);
    setInput("");
    setIsLoading(true);

    try {
      // Send this input to the API to get a response.
      const response = await fetch("/api/get-answer", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({ question: input }),
      });

      if (!response.ok) {
        throw new Error("Network response was not ok");
      }

      const data = await response.json();
      const botResponse = data.answer.result; // API 응답에서 result 값을 가져옵니다.

      setMessages((prevMessages) => [
        ...prevMessages,
        { text: botResponse, type: "bot" },
      ]);
    } catch (error) {
      setMessages((prevMessages) => [
        ...prevMessages,
        { text: "서버와의 통신 중 오류가 발생했습니다.", type: "bot" },
      ]);
    } finally {
      setIsLoading(false);
    }
  };

  return (
    <div className="h-screen flex flex-col ">
      <div className={` ${styles.flexCenter} my-auto`}>
        <div className={` flex-grow  overflow-y-auto space-y-40`}>
          {messages.length === 0 ? (
            <div
              className={`${styles.flexCenter} w-[100%]  max-w-3xl xl:max-w-4xl mx-auto`}
            >
              <div className="flex flex-col w-full space-y-16">
                <div className="flex flex-row space-x-7">
                  <div className="flex flex-col">
                    <div className="flex flex-row bg-white   text-white py-4 pr-4 pl-2 ">
                      <img
                        src="/icons/symbol.png"
                        className="w-[30px] h-[30px] mr-2"
                      />
                      <h1 className="font-semibold leading-7 text-[18px] text-black mr-3">
                        ChatPyeongTaek
                      </h1>
                      <div
                        className=" text-light rounded-lg border-[1px]
                        font-semibold border-solid bg-[#F9FAFB] px-2 py-0.5
                        text-[#9ca3af] "
                      >
                        {" "}
                        v1.2b
                      </div>
                    </div>
                    <p className="text-[16px] pl-2 text-black">
                      한국어 LM으로 만든 평택대학교 서비스 <br /> ChatPyeongTaek
                    </p>
                  </div>
                  {/* <div
                    className={`${styles.flexStart} text-start sm:pt-4 space-y-6 rounded-xl border w-[50%] flex-col`}
                  >
                    <p className="px-4 text-sm">
                      Using Model <br />
                      ChatGPT OpenSource
                    </p>
                    <div
                      className={`rounded-xl bg-[#F3F4F6] h-full px-4 ${styles.flexCenter} flex-row space-x-14`}
                    >
                      <p className="text-[14px]">Model</p>
                      <p className="text-[14px]">Datest</p>
                      <p className="text-[14px]">Github</p>
                    </div>
                  </div> */}
                </div>
                <div className="flex flex-col">
                  <div className="pb-3 pl-2">Examples</div>
                  <div className="flex flex-row space-x-8 pl-2">
                    <div
                      className={`${styles.flexCenter} text-center sm:p-4 rounded-xl border w-[50%]`}
                    >
                      "평택대학교 위치가 어디야"
                    </div>
                    <div
                      className={`${styles.flexCenter} text-center  sm:p-4 rounded-xl border w-[50%]`}
                    >
                      "데이터정보학과에 대해 알려주세요"
                    </div>
                    <div
                      className={`${styles.flexCenter} text-center sm:p-4 rounded-xl border w-[50%]`}
                    >
                      "휴학신청은 어디서 하는지 알려주세요"
                    </div>
                  </div>
                </div>
              </div>
            </div>
          ) : (
            <div className="flex flex-col overflow-y-auto space-y-4 p-4 mb-4">
              {messages.map((message, idx) => (
                <div
                  key={idx}
                  className={`w-4/5 mx-auto ${
                    message.type === "user" ? "self-end" : "self-start"
                  }`}
                >
                  <span
                    className={`inline-block w-full px-4 py-2 rounded-lg ${
                      message.type === "user"
                        ? "bg-blue-500 text-white rounded-br-none"
                        : "bg-gray-200 text-black rounded-bl-none"
                    }`}
                  >
                    {message.text}
                  </span>
                </div>
              ))}
              <div ref={messagesEndRef} />
            </div>
          )}
          <div className={`flex mx-[10%] border rounded-xl`}>
            <textarea
              ref={textAreaRef}
              value={input}
              onChange={handleInputChange}
              onKeyPress={handleKeyPress}
              className="flex-1 px-4 py-3 rounded-l-xl bg-gray-100 text-black focus:outline-none"
              style={{
                minHeight: "20px",
                height: "40px",
                maxHeight: "180px",
                // 스크롤바 숨기기
                resize: "none", // 크기 조절 불가능하게 설정
                lineHeight: "1.5", // 행간 설정
              }}
            />
            <div
              onClick={!isLoading ? sendMessage : null} // 로딩 중이 아닐 때만 sendMessage 함수를 호출
              className={`${styles.flexCenter} rounded-r-xl bg-gray-100 text-black px-4 py-2  `}
            >
              {!isLoading ? (
                <FontAwesomeIcon
                  icon={faPaperPlane}
                  style={{ color: "black" }}
                  className="w-[20px] h-[20px] cursor-pointer"
                />
              ) : (
                <FontAwesomeIcon
                  icon={faSpinner}
                  className="fa-spin w-[20px] h-[20px]"
                />
              )}
            </div>
          </div>
        </div>
      </div>
    </div>
  );
};

export default Chatbot;
