# CalDAV Framework

## Description

This framework abstracts all VCALENDAR entities. In addition it automatically adds required timezone information for dates. You can simply work with PHP DateTime and DateTimeZone objects.

After creating an event or a todo and setting all required values, you can create the vcalendar output or save the entry directly.

## Example usage

```php
require_once('autoload.php');

try {
    $client = new \Brainformatik\CalDAV\Connection\Client([
        'baseUri' => 'http://some-url' // set caldav server url
    ]);

    $calendar = new \Brainformatik\CalDAV\Entity\Calendar($client, 'http://url-to-calendar');

    $event = $calendar->addEvent();
    $event->setUid('unique-id123415');
    $event->setSummary('This is the summary');
    $event->setDescription('This is the description which can be very long and should be folded at some point if the implementation is working correctly.');
    $event->setComment('This is the comment you can use for something.');
    $event->setLocation('Meeting room 1');
    $event->setPriority(9);
    $event->addResources(['BEAMER', 'Flipchart']);
    $event->setStatus(\Brainformatik\CalDAV\Enum\EventStatus::CONFIRMED);
    $event->setDateEnd(new DateTime('2016-01-01 14:00:10', new DateTimeZone('America/New_York')));
    $event->setDateStart(new DateTime('2015-12-12', new DateTimeZone('America/New_York')));
    $event->setRecurrenceId(new DateTime('2015-12-12', new DateTimeZone('America/New_York')), [
        'RANGE' => 'THISANDFUTURE',
        'VALUE' => 'DATE'
    ]);

    $duration = new \Brainformatik\CalDAV\Type\Duration();
    $duration->setHour(10)->setMinute(30);

    $event->setDuration($duration);
    $event->setTransparency(\Brainformatik\CalDAV\Enum\EventTransparency::TRANSPARENT);
    $event->addExceptionDates([
        new DateTime('2015-12-12 12:00:00', new DateTimeZone('America/New_York')),
        new DateTime('2015-12-13 12:00:00', new DateTimeZone('America/New_York')),
        new DateTime('2015-12-14 12:00:00', new DateTimeZone('Europe/Berlin'))
    ]);

    $event->setDateCreated(new DateTime('2016-10-08 09:32:00', new DateTimeZone('UTC')));
    $event->setDateLastModified(new DateTime('2016-10-08 09:32:00', new DateTimeZone('UTC')));

    // to get the icalendar output
    echo $calendar->serialize();
    
    // to save the event to the calendar
    $calendar->save('filename.ics');
} catch (\Exception $e) {
    echo $e->getMessage();
}
```