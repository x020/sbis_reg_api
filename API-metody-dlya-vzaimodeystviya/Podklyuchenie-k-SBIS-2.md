Подключение к СБИС 2
Алгоритм подключения клиента к системе электронной отчётности СБИС2

    Завести организацию в базе Биллинга, если её ещё нет, и определить её идентификатор:
    [Contractor.FindOrCreate](../Rabota-s-klientami/Contractor-FindOrCreate.md)(Реквизиты_контрагента)

    Определить список аккаунтов, в которые входит данная организация:
    [Contractor.InfoByID](../Rabota-s-klientami/Contractor-InfoByID.md)(ID_контрагента)

    Если в этом списке нет подходящего аккаунта, создать новый:
    [Contractor.CreateWithAccount](../Rabota-s-klientami/Contractor-CreateWithAccount.md)(Реквизиты_Контрагента, True)

    Если необходимо, добавить в выбранный аккаунт другие организации:
    [Account.AddContractor](../Rabota-s-klientami/Account-AddContractor.md)(ID_аккаунта, Реквизиты_контрагента)
    или, если контрагент с указанными реквизитами уже есть в Биллинге и его идентификатор известен:
    [Account.AddContractorByID](../Rabota-s-klientami/Account-AddContractorByID.md)(ID_аккаунта, ID_контрагента)

    Создать лицензию для аккаунта с требуемым набором услуг:
    [Account.CreateOrEditLicense](../Rabota-s-licenziyami/Account-CreateOrEditLicense.md)(ID_аккаунта, Данные_лицензии)
    Для того чтобы узнать, какие услуги могут быть подключены, можно воспользоваться методом:
    [License.AvailableServices](../Prais-deystvuyushchih-uslug/License-AvailableServices.md)(Код_региона).

    Активировать эту лицензию:
    [License.Activate](../Rabota-s-licenziyami/License-Activate.md)(ID_лицензии, Дата_активации)
    Далее для каждой организации в аккаунта, которую нужно подключить к электронной отчётности, необходимо выполнить следующие шаги.

    Добавить сертификат для подписи документов:
    [Contractor.AddCertificate](../Rabota-s-gosorganami/Contractor-AddCertificate.md)(ID_аккаунта, ID_контрагента, Файл_сертификата)

    Добавить направления сдачи в те госорганы, в которые предполагается сдача отчётности:
    [Contractor.AttachToInspection](../Rabota-s-gosorganami/Contractor-AttachToInspection.md)(ID_контрагента, Тип_инспекции, Код_инспекции, Доп_код_инспекции).

    Проверить состояние регистрации в госорганах можно с помощью следующего метода:
    [Contractor.InspectionList](../Rabota-s-gosorganami/Contractor-InspectionList.md)(ID_контрагента)
    После того, как контрагент будет зарегистрирован в том или ином госоргане, он может использовать систему электронной отчётности СБИС2 для отправки отчётов в этот госорган.
