# XiaoheRime
这是一个在 rime-ice 基础上修改得来的小鹤双拼配置方案。增加了鹤形码、虎码和音调辅码。

本项目基于[iDvel/rime-ice: Rime 配置：雾凇拼音 | 长期维护的简体词库](https://github.com/iDvel/rime-ice)修改，原项目遵循 GNU GPLv3 许可协议。本项目的所有修改部分同样遵循 GNU GPLv3 许可协议。

修改部分如下：

- lua/pin_card_filter：
  - 新增代码块，将键盘输入的内容置顶为输入法选框的首选项。
  ```
  -- 获取当前输入框内容（preedit）
    local preedit = env.engine.context:get_preedit().text
    -- 只保留英文字母（如果你只想置顶字母串）
    local letter_only = preedit:gsub("[^a-zA-Z]", "")
    -- 如果有输入内容，就置顶
    if #letter_only > 0 then
        local always_top = Candidate("word", 0, 0, letter_only, "")
        yield(always_top)
    end
    -- 继续输出其他候选
    for cand in input:iter() do
        yield(cand)
    end
  ```
- 新增 custom_phrase_double.txt：
  - 作为自定义编码的词库，其内容为[Ryan-the-hito/XiaoheSougou: A full form-based character code bundle for Xiaohe Chinese input method. 本库为 macOS 上搜狗输入法小鹤双拼（音形）的挂载词库（自用）](https://github.com/Ryan-the-hito/XiaoheSougou)中配置的码表，具体内容包括：
  	- 小鹤双拼四字词语
  	- 小鹤音形单字
  	- 虎码单字
- default.yaml：
  - 注释掉了除小鹤双拼之外的其他输入方式
  - 候选词从 5 个提升到了 9 个
  - 允许 ASDFGHJKL 编码，数字键被虎码编码占用
- double_pinyin_flypy.schema.yaml：
  - 在 translator 中增添辅码
  ```
  table_translator@fuma
  ```
  - 修改了主翻译器、次翻译器、自定义短语以及辅码的初始权重：辅码>主翻译器（中文）>自定义短语>次翻译器（英文）>中英混合词汇
  - 新增快捷键，修改了既有的上屏原则
    - 更改前：数字、空格上屏，回车输入按键源码
    - 更改后：数字用于编码，不可上屏；空格用于向右移动候选词，Shift+空格向左移动候选词；回车上屏
    ```
    bindings:
    - { when: composing, accept: space, send: Right }
    - { when: composing, accept: Shift+space, send: Left }
    - { when: composing, accept: Return, send: Shift+Control+Return }
    ```
  - 拼写设定中新增数字键用于编码
  - 新增辅码相关配置
- 新增 fuma.txt：
	- 辅码词库，原始文件亦来自于[Ryan-the-hito/XiaoheSougou: A full form-based character code bundle for Xiaohe Chinese input method. 本库为 macOS 上搜狗输入法小鹤双拼（音形）的挂载词库（自用）](https://github.com/Ryan-the-hito/XiaoheSougou)中的码表，具体为音调辅码（所有可显汉字库+多音字优化）
- squirrel.yaml：
  - 将排列方式改为横向排列
  - 新增模仿原生输入法的皮肤设置
