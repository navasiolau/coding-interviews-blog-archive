Serialize and Deserialize Binary Tree
=====================================

Sep 28, 2020 · 2 min read

Сериализация — процесс представления объектов в памяти в виде набора битиков, так чтобы потом можно было передать по сети или записать на диск и распарсить назад в исходные объекты.

В этой задаче нужно сериализовать и десериализовать бинарное дерево.

[Задача на LeetCode](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/).

Решение [#](#решение)
---------------------

Отличная задачка на рекурсию. Будем сериализовывать в строку (для экономии памяти выбирают бинарный формат, но для этой задачи лучше взять текстовый — для читаемости).

Чтобы сериализовать дерево его нужно как-то обойти, есть разные варианты — подробнее я писал в [разборе задачи про обход дерева](https://www.notion.so/N-ary-Tree-Preorder-Traversal-9a0c58cfd47b42898bf48ec056904acb).

Возьмём preorder вариант обхода, соберём все значения в массив, склеим массив в строчку с определённым разделителем (чтобы потом можно было распарсить назад). Вот и вся сериализация.

    /**
     * Сеарилизует бинарное дерево в строку.
     *
     * @param {TreeNode} root
     * @return {string}
     */
    var serialize = function(root) {
      const result = [];
      // удобно собрать все значения в result,
      // который передаётся по ссылке через все рекурсивные вызовы,
      // а после склеим всё в строку
      serializeHelper(root, result);
      return result.join('|');
    };
    
    /**
     * @param {TreeNode} root
     * @param {String[]} result
     * @return {void}
     */
    function serializeHelper(root, result) {
      // базовый случай — самый низ ветки дерева,
      // дальше идти некуда, добавим определённый символ «end of tree».
      // Выбрать можно любой символ.
      if (root === null) {
        result.push('$');
        return;
      }
      // preorder вариант обхода,
      // собираем значение, идём сперва в левую, потом в правую ветки — поиск в глубину
      result.push(root.val);
      serializeHelper(root.left, result);
      serializeHelper(root.right, result);
    }
    

Рассмотрим обратный процесс — как из строки получить исходное дерево. Идём от обратного:

*   делим строчку по разделителю, получаем значения
*   начинаем создавать узлы дерева, рекурсивно обходя массив

    /**
     * Десериализует строку назад в дерево.
     *
     * @param {string} data
     * @return {TreeNode}
     */
    var deserialize = function(data) {
      // разворачиваем массив — получим стек,
      // удобно снимать значения по одному с вершины стека
      return deserializeHelper(data.split('|').reverse());
    };
    
    function deserializeHelper(vals) {
      // вершина стека (pop одновременно и снимает элемент)
      const val = vals.pop();
      // тот самый спецсимвол «конец ветки дерева»
      if (val === '$') {
        return null;
      }
      // создаём новый узел со значением на вершине стека
      const node = new TreeNode(val);
      // дергаем рекурсию в том же порядке, что и при сериализации (preorder)
      node.left = deserializeHelper(vals);
      node.right = deserializeHelper(vals);
      return node;
    }
    

Сложность решения — линейная относительно количества узлов, как по времени, так и по памяти, т.к. необходимо обойти все узлы дерева.

![](/images/serialize-and-deserialize-binary-tree--result.jpg)

PS. Обсудить можно в [телеграм-чате](https://t.me/ctci_chat_ru) любознательных программистов. Welcome! 🤗

Подписывайтесь на [мой твитер](https://twitter.com/vitkarpov) или [канал в телеграме](https://t.me/coding_interviews), чтобы узнавать о новых разборах задач.