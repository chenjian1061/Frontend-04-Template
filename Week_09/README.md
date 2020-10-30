## 第九周学习总结 ##


----------
## 1.CSS总论 | CSS语法的研究 ##
 - CSS2.1的语法
    - https://www.w3.org/TR/CSS21/grammar.html#q25.0
    - https://www.w3.org/TR/css syntax 3
 - CSS总体结构
    - @charset
    - @import
    - rules
        - @media
        - @page
        - rule
 - CSS知识体系
    - CSS
        - at-rules
            - @charset
            - @import
            - **@media**
            - @page
            - @counter-style
            - **@keyframes**
            - **@fontface**
            - @supports
            - @namespace
        - rule
            - Selector
                - selector_group
                - selector
                    - >
                    - <sp>
                    - +
                    - ~
                - simple_selector
                    - type
                    - *
                    - .
                    - #
                    - []
                    - :
                    - ::
                    - :not()
            - Declaration
                - Key
                    - variables
                    - properties
                - Value
                    - calc
                    - number
                    - length
                    - ......
        
## 2.CSS总论 | CSS@规则的研究 ##
- At-rules
    - @charset https://www.w3.org/TR/css-syntax-3/
    - @import https://www.w3.org/TR/css-cascade-4/
    - @media https://www.w3.org/TR/css3-conditional/
    - @page https://www.w3.org/TR/css-page-3/
    - @counter-style https://www.w3.org/TR/css-counter-styles-3
    - @keyframes https://www.w3.org/TR/css-animations-1/
    - @fontface https://www.w3.org/TR/css-fonts-3/
    - @supports https://www.w3.org/TR/css3-conditional/
    - @namespace https://www.w3.org/TR/css-namespaces-3/
    
## 3.CSS总论 | CSS规则的结构 ##
- CSS规则
    - 选择器
    - 声明
        - Key
        - Value
- 标准
    - Selector
        - https://www.w3.org/TR/selectors-3/
        - https://www.w3.org/TR/selectors-4/
    - Key
        - Properties
        - Variables: https://www.w3.org/TR/css-variables/
    - Value
        - https://www.w3.org/TR/css values-4/

## 4.CSS总论 | 收集标准 ##
## 5.CSS总论 | CSS总论总结 ##
- CSS语法
- at-rule
- seletor
- variables
- value
- 实验

## 6.CSS选择器 | 选择器语法 ##
- 简单选择器
    - *
    - div svg|a
    - .cls
    - #id
    - [attr=value]
    - :hover
    - ::before
- 复合选择器
    - <简单选择器><简单选择器><简单选择器>
    - *或者div必须写在前面
- 复杂选择器
    - <复合选择器><sp><复合选择器>
    - <复合选择器>">"<复合选择器>
    - <复合选择器>"~"<复合选择器>
    - <复合选择器>"+"<复合选择器>
    - <复合选择器>"||"<复合选择器>

## 7.CSS选择器 | 选择器的优先级 ##
- 简单选择器计数

## 8.CSS选择器 | 伪类 ##
- 链接/行为
    - :any-link
    - :link :visited
    - :hover
    - :active
    - :focus
    - :target
-树结构
    - :empty
    - :nth-child()
    - :nth-last-child()
    - :first-child :last-child :only-child
- 逻辑型
    - :not伪类
    - :where :has

## 9.CSS选择器 | 伪元素 ##
- ::before
- ::after
- ::first-line
- ::first-letter