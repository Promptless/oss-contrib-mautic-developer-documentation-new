Mail helper
###########

.. vale off

.. note::

   The content for this page requires a major update. The legacy page contains outdated and potentially inaccurate information. You can still access it in the :xref:`legacy repository`.

   If you're interested in helping develop the new content for this page and others, consider joining the documentation efforts.

   Please read the :xref:`dev docs contributing guidelines` and :xref:`Contributing to Mautic’s documentation` to get started.

.. vale on

Use ``Mautic\EmailBundle\Helper\MailHelper`` to send Email. The helper runs content through Mautic's event listeners, so tokens are replaced and the content is manipulated the same way it is for regular Mautic Emails. Inject the helper and call ``getMailer()`` to get a configurable instance:

.. code-block:: php

    <?php

    use Mautic\EmailBundle\Helper\MailHelper;

    final class ExampleService
    {
        public function __construct(private MailHelper $mailHelper)
        {
        }

        public function sendMail(array $email, string $content): void
        {
            $mailer = $this->mailHelper->getMailer();

            // To address; you can also use setTo(), addCc(), setCc(), addBcc(), or setBcc()
            $mailer->addTo('contact@example.com', 'Example Contact');

            // Set a custom from address; the system settings are used by default
            $mailer->setFrom('sender@example.com', 'Example Sender');

            // Set the subject
            $mailer->setSubject($email['subject']);

            // Set the content
            $mailer->setBody($content);
            $mailer->parsePlainText($content);

            // Send the Email; pass true to dispatch through the event listeners
            if ($mailer->send(true)) {
                // Optionally create a stat to allow a web view, tracking, and so on
                $mailer->createLeadEmailStat();
            } else {
                $errors           = $mailer->getErrors();
                $failedRecipients = $errors['failures'];
            }
        }
    }

Sending batches with token support
**********************************

Some transports support tokenized Emails for multiple recipients. When sending the same Email to many Contacts, enable batch mode with ``enableQueue()`` and send with ``flushQueue()`` in place of ``send()``:

.. code-block:: php

    <?php

    $mailer = $this->mailHelper->getMailer();
    $failed = [];

    $mailer->enableQueue();

    foreach ($emailList as $email) {
        if (!$mailer->addTo($email['email'], $email['name'])) {
            // Clear the errors so they don't stop the next send
            $mailer->clearErrors();

            $failed[] = $email;
        }
    }

    // Flush the pending queue
    if (!$mailer->flushQueue()) {
        $errors = $mailer->getErrors();
        $failed = array_merge($failed, $errors['failures']);
    }

If you're working with an Email entity - ``Mautic\EmailBundle\Entity\Email`` - pass it to ``$mailer->setEmail($email)`` and the subject, body, and Assets are extracted and set automatically.

Attachments
***********

Attach files with ``attachFile()``, or attach a Mautic Asset - ``Mautic\AssetBundle\Entity\Asset`` - with ``attachAsset()``. Refer to the ``MailHelper`` class for the full list of available methods.
