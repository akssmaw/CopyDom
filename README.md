# CopyDom
var person = prompt("请输入节点", "");
			var myDiv = eval(person);
			String.prototype.hashCode = function() {
				var hash = 0,
					i, chr;
				if (this.length === 0) return hash;
				for (i = 0; i < this.length; i++) {
					chr = this.charCodeAt(i);
					hash = ((hash << 5) - hash) + chr;
					hash |= 0;
				}
				return hash;
			};
			let hashCodeList = [];
			let addElement = document.createElement("div")
			console.log(forElement(myDiv, 0));

			function forElement(elements, eleIndex) {
				let newElements = elements.children
				for (let index = 0; index < newElements.length; index++) {
					let eleKeyChildren = newElements[index]
					let typeOfs = typeof(eleKeyChildren)
					let eleKeyChildrenCode = eleKeyChildren.outerHTML
					eleKeyChildren.setAttribute("getClass", "hashCodes" + eleKeyChildrenCode.hashCode())

					hashCodeList.push(eleKeyChildrenCode.hashCode())
					if (typeOfs == "object") {
						if (eleKeyChildren.children.length > 0) {
							forElement(eleKeyChildren, index)
						} else if (eleKeyChildren.children.length == 0) {}
					} else {}
				}
			};
			let arrBeJson = "";
			for (let index in hashCodeList) {
				let code = hashCodeList[index]
				let selectAllCode = '[getclass="hashCodes' + code + '"]'
				var nodes = document.querySelectorAll(selectAllCode)[0];
				var computedStyles = window.getComputedStyle(nodes);
				let beJsonAdd = `${selectAllCode}{`
				for (var i = 0; i < computedStyles.length; i++) {
					var propertyName = computedStyles[i];
					var propertyValue = computedStyles.getPropertyValue(propertyName);
					beJsonAdd += propertyName + ": " + propertyValue + ";";
				}
				beJsonAdd += "}";
				arrBeJson += beJsonAdd;
			};
			console.log(arrBeJson);
			console.log(myDiv.outerHTML);
			let htmlDom =
				`<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<style>
		${arrBeJson}

		</style>
	<body>
		${myDiv.outerHTML}

	</body>
</html>`;
			saveTextToFile(htmlDom, "index.html");

			function saveTextToFile(text, filename) {
				const blob = new Blob([text], {
					type: 'text/plain'
				});
				const a = document.createElement('a');
				a.href = URL.createObjectURL(blob);
				a.download = filename;
				a.click();
				URL.revokeObjectURL(a.href);
			};
