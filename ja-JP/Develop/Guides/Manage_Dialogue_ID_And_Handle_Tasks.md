# ダイアログIDを管理し、作業を処理する

[間接ダイアログ](/Develop/CIC_Overview.md#IndirectDialogue)の問題を解決するために、**ダイアログID**という概念を使用します。クライアントは、ダイアログIDに関連して、以下の内容を実行する必要があります。

* [ダイアログIDを作成する](#CreatingDialogueID)
* [ダイアログIDによってディレクティブを処理する](#HandleDirectivesByDialogueID)

## ダイアログIDを作成する {#CreatingDialogueID}

**ダイアログID**とは、ユーザーのリクエストを識別するために、ユーザーが発話を開始する度に作成される識別子です。クライアントは、ユーザーのリクエストをCICに送信する度に、最終リクエストのダイアログIDを保管する必要があります。これを**最終ダイアログID**といいます。**最終ダイアログID**とは、クライアントがCICに最終的に送信した[SpeechRecognizer.Recognize](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize)イベント、または[TextRecognizer.Recognize](/Develop/References/MessageInterfaces/TextRecognizer.md#Recognize)イベントに含まれたダイアログIDのことです。クライアントは、この最終ダイアログIDを保存し、CICにリクエストを送信する度に更新する必要があります。

クライアントは、次のように動作する必要があります。

![](/Develop/Assets/Images/CIC_Dialogue_ID_Creation.svg)

<ol>
  <li> ユーザーが新しい対話を開始する度に<strong>新しいダイアログIDを作成</strong>します（UUID形式をお勧めします）。</li>
  <li><a href="/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize">SpeechRecognizer.Recognize</a>イベントでユーザーのリクエストをCICに送信します。テキストのリクエストは、<a href="/Develop/References/MessageInterfaces/TextRecognizer.md#Recognize">TextRecognizer.Recognize</a>イベントを使用します。
    <ul>
      <li>そのとき<a href="/Develop/References/CIC_API.md#Event">イベントのヘッダー</a>の<code>dialogRequestId</code>に新しく作成したダイアログIDを含めます。</li>
    </ul>
  </li>
  <li>イベントを送信すると、作成したダイアログIDを<strong>最終ダイアログID</strong>として保管します。</li>
</ol>

<div class="note">
<p><strong>メモ</strong></p>
<p>最終ダイアログIDは、<strong><a href="/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize">SpeechRecognizer.Recognize</a>イベント、または<a href="/Develop/References/MessageInterfaces/TextRecognizer.md#Recognize">TextRecognizer.Recognize</a>イベントの送信を完了した後、必ず更新</strong>される必要があります。最終ダイアログIDを保存・更新しないと、ユーザーの急なリクエストの変更に対応できないことがあります。詳細については、<a href="/Develop/CIC_Overview.md#IndirectDialogue">間接ダイアログ</a>を参照してください。</p>
</div>

最終ダイアログIDを更新した後、クライアントは、新しい[ダイアログIDを持つディレクティブを処理する](#HandleDirectivesByDialogueID)ために、以下を実行する必要があります。

* 前のダイアログIDを持つディレクティブの内容をユーザーに提供している場合、[オーディオ再生の基本ルール](/Design/Audio.md#AudioInterruptionRule)または[ユーザー発話時のオーディオ再生ルール](/Design/Audio.md#AudioInterruptionRuleForUserUtterance)を参照して、それを中止する必要があります。
* [メッセージキュー](/Develop/Guides/Interact_with_CIC.md#ManageMessageQ)から以前のダイアログIDを持つディレクティブをすべて破棄する必要があります。

## ダイアログIDによってディレクティブを処理する {#HandleDirectivesByDialogueID}

通常、CICは、リクエストに対する応答としてクライアントにディレクティブを送信します。そのディレクティブに、[クライアントが作成したダイアログID](#CreatingDialogueID)が含まれます。クライアントはダイアログIDから、CICから受信した結果が現在のリクエストに適切な応答かどうかを確認できます。クライアントは、ダイアログIDによって、以下のようにディレクティブを処理する必要があります。

![](/Develop/Assets/Images/CIC_Handle_Directives_By_Dialogue_ID.svg)

最初に、クライアントはCICから受信した[ディレクティブのヘッダー」(/Develop/References/CIC_API.md#Directive)にダイアログIDが含まれているかを確認します。受信したディレクティブにダイアログIDが含まれている場合、**最終ダイアログID**と比較し、その結果によって以下のように処理します。

* **2つのダイアログIDが一致する場合**、受信したメッセージを[メッセージキュー](/Develop/Guides/Interact_with_CIC.md#ManageMessageQ)に追加し、順番になると、その内容をユーザーに提供します。
* **ダイアログIDが一致しない場合**、受信したディレクティブを破棄します。

**ディレクティブにダイアログIDが含まれていない場合**、クライアントは**即座に**受信したディレクティブを処理します。ダイアログIDを持たないディレクティブは、主に[Downchannel](/Develop/References/CIC_API.md#EstablishDownchannel)で送信され、対話に割り込まないバックグラウンドのサービスを指示するものです。
